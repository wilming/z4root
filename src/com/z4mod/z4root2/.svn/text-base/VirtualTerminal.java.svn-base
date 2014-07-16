package com.z4mod.z4root2;

import java.io.ByteArrayOutputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStream;

import android.util.Log;

public class VirtualTerminal {

	Process process;
	DataOutputStream toProcess;
	final Object ReadLock = new Object();
	final Object WriteLock = new Object();
	
	ByteArrayOutputStream inpbuffer = new ByteArrayOutputStream();
	ByteArrayOutputStream errbuffer = new ByteArrayOutputStream();
	
	InputReader inpreader;
	InputReader errreader;

	public VirtualTerminal() throws IOException, InterruptedException {
		process = Runtime.getRuntime().exec("su");
		toProcess = new DataOutputStream(process.getOutputStream());
		
		inpreader = new InputReader(process.getInputStream(), inpbuffer);
		errreader = new InputReader(process.getErrorStream(), errbuffer);
		
		Thread.sleep(10);
		
		inpreader.start();
		errreader.start();
	}
	
	public VTCommandResult runCommand(String command) throws Exception {
		Log.i("oclf", command);
		synchronized (WriteLock) {
			inpbuffer.reset();
			errbuffer.reset();
		}
    	
        toProcess.writeBytes(command + "\necho :RET=$?\n");
        toProcess.flush();
        while (true) {
            synchronized (ReadLock) {
            	boolean doWait;
            	synchronized (WriteLock) {
            		byte[] inpbyte = inpbuffer.toByteArray();
            		String inp = new String(inpbyte);
            		doWait = !inp.contains(":RET=");
            	}
            	if (doWait) {
            		ReadLock.wait();
            	}
            }
            synchronized (WriteLock) {
            	byte[] inpbyte = inpbuffer.toByteArray();
            	byte[] errbyte = errbuffer.toByteArray();
            	
            	String inp = new String(inpbyte);
            	String err = new String(errbyte);
            	
            	if (inp.contains(":RET=")) {
            		if (inp.contains(":RET=EOF") || err.contains(":RET=EOF"))
            			throw new BrokenPipeException();
                	if (inp.contains(":RET=0")) {
                		Log.i("oclf success", inp);
                		return new VTCommandResult(0, inp, err);
                	} else {
                		Log.i("oclf error", err);
                		return new VTCommandResult(1, inp, err);
                	}
            	}
            }
        }
	}
	
	// Fire n Forget
	public void FNF(String command) throws Exception {
        toProcess.writeBytes(command + "\n");
        toProcess.flush();
	}
	
	public void shutdown() {
		inpreader.interrupt();
		errreader.interrupt();
		process.destroy();
	}
	
	public class InputReader extends Thread {
		
		InputStream is;
		ByteArrayOutputStream baos;
		
		public InputReader(InputStream is, ByteArrayOutputStream baos) {
			this.is = is;
			this.baos = baos;
		}
		
		@Override
		public void run() {
			try {
				byte[] buffer = new byte[1024];
				while (true) {
					int read = is.read(buffer);
					if (read < 0) {
						synchronized(WriteLock) {
							buffer = ":RET=EOF".getBytes();
							baos.write(buffer);
						}
						synchronized (ReadLock) {
							ReadLock.notifyAll();
						}
						return;
					}
					if (read > 0) {
						synchronized(WriteLock) {
							baos.write(buffer, 0, read);
						}
						synchronized (ReadLock) {
							ReadLock.notifyAll();
						}
					}
				}
			} catch (Exception ex) {
				ex.printStackTrace();
			}
		}
	}

	public class VTCommandResult {
		public final String stdout;
		public final String stderr;
		public final Integer exit_value;

		VTCommandResult(Integer exit_value_in, String stdout_in, String stderr_in) {
			exit_value = exit_value_in;
			stdout = stdout_in;
			stderr = stderr_in;
		}

		VTCommandResult(Integer exit_value_in) {
			this(exit_value_in, null, null);
		}

		public boolean success() {
			return exit_value != null && exit_value == 0;
		}
	}
}
