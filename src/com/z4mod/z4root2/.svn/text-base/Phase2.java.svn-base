package com.z4mod.z4root2;

import jackpal.androidterm.Exec;

import java.io.FileDescriptor;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.zip.GZIPInputStream;

import android.app.Activity;
import android.content.Context;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.os.PowerManager;
import android.os.PowerManager.WakeLock;
import android.util.Log;
import android.widget.TextView;

public class Phase2 extends Activity {
	
	TextView detailtext;
	int MODE;
	
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

		SharedPreferences settings = getSharedPreferences(z4root.PREFS_NAME, 0);
		MODE = settings.getInt(z4root.PREFS_MODE, 0);
		
		setContentView(R.layout.p2);
		
		detailtext = (TextView) findViewById(R.id.detailtext);
		
		new Thread() {
			public void run() {
				if (MODE == z4root.MODE_UNROOT) {
					dounroot();
				} else if (MODE == z4root.MODE_TEMPROOT) {
					dotemproot();
				} else {
					dopermroot();
				}
			};
		}.start();	
		
	}
	
	public void saystuff(final String stuff) {
		runOnUiThread(new Runnable() {
			
			@Override
			public void run() {
				detailtext.setText(stuff);
			}
		});
	}
	
	public void dounroot() {
		PowerManager pm = (PowerManager) getSystemService(POWER_SERVICE);
		final WakeLock wl = pm.newWakeLock(PowerManager.FULL_WAKE_LOCK | PowerManager.ACQUIRE_CAUSES_WAKEUP | PowerManager.ON_AFTER_RELEASE, "z4root");
		wl.acquire();
		Log.i("AAA", "Starting");

		final int[] processId = new int[1];
		final FileDescriptor fd = Exec.createSubprocess("/system/bin/sh", "-", null, processId);
		Log.i("AAA", "Got processid: " + processId[0]);

		final FileOutputStream out = new FileOutputStream(fd);
		final FileInputStream in = new FileInputStream(fd);


		new Thread() {
			public void run() {
				byte[] mBuffer = new byte[4096];
				int read = 0;
				while (read >= 0) {
					try {
						read = in.read(mBuffer);
						String str = new String(mBuffer, 0, read);
						//saystuff(str);
						Log.i("AAA", str);
					} catch (Exception ex) {
						
					}
				}
				wl.release();
			}
		}.start();

		try {
			write(out, "id");
			try {
				SaveIncludedZippedFileIntoFilesFolder(R.raw.busybox, "busybox", getApplicationContext());
			} catch (Exception e1) {
				e1.printStackTrace();
			}
			write(out, "chmod 777 " + getFilesDir() + "/busybox");
			write(out, getFilesDir() + "/busybox mount -o remount,rw /system");
			write(out, getFilesDir() + "/busybox rm /system/bin/su");
			write(out, getFilesDir() + "/busybox rm /system/xbin/su");
			write(out, getFilesDir() + "/busybox rm /system/bin/busybox");
			write(out, getFilesDir() + "/busybox rm /system/xbin/busybox");
			write(out, getFilesDir() + "/busybox rm /system/app/SuperUser.apk");
			write(out, "echo \"reboot now!\"");
			saystuff("Rebooting...");
			Thread.sleep(3000);
			write(out, "sync\nsync");
			write(out, "reboot");
		} catch (Exception ex) {
			ex.printStackTrace();
		}
	}
	
	public void dotemproot() {
		PowerManager pm = (PowerManager) getSystemService(POWER_SERVICE);
		final WakeLock wl = pm.newWakeLock(PowerManager.FULL_WAKE_LOCK | PowerManager.ACQUIRE_CAUSES_WAKEUP | PowerManager.ON_AFTER_RELEASE, "z4root");
		wl.acquire();
		Log.i("AAA", "Starting");

		final int[] processId = new int[1];
		final FileDescriptor fd = Exec.createSubprocess("/system/bin/sh", "-", null, processId);
		Log.i("AAA", "Got processid: " + processId[0]);

		final FileOutputStream out = new FileOutputStream(fd);
		final FileInputStream in = new FileInputStream(fd);


		new Thread() {
			public void run() {
				byte[] mBuffer = new byte[4096];
				int read = 0;
				while (read >= 0) {
					try {
						read = in.read(mBuffer);
						String str = new String(mBuffer, 0, read);
						//saystuff(str);
						Log.i("AAA", str);
						if (str.contains("finished checked")) {
							saystuff("Temporary root applied! You are now rooted until your next reboot.");
							break;
						}
					} catch (Exception ex) {						
					}
				}
				wl.release();
			}
		}.start();

		try {
			write(out, "id");
			try {
				SaveIncludedZippedFileIntoFilesFolder(R.raw.busybox, "busybox", getApplicationContext());
				SaveIncludedZippedFileIntoFilesFolder(R.raw.su, "su", getApplicationContext());
				SaveIncludedZippedFileIntoFilesFolder(R.raw.superuser, "SuperUser.apk", getApplicationContext());
			} catch (Exception e1) {
				e1.printStackTrace();
			}
			write(out, "chmod 777 " + getFilesDir() + "/busybox");
			write(out, getFilesDir() + "/busybox killall rageagainstthecage");
			write(out, getFilesDir() + "/busybox killall rageagainstthecage");
			write(out, getFilesDir() + "/busybox rm "+getFilesDir()+"/temproot.ext");
			write(out, getFilesDir() + "/busybox rm -rf "+getFilesDir()+"/bin");
			write(out, getFilesDir() + "/busybox cp -rp /system/bin "+getFilesDir());
			write(out, getFilesDir() + "/busybox dd if=/dev/zero of="+getFilesDir()+"/temproot.ext bs=1M count=15");
			write(out, getFilesDir() + "/busybox mknod /dev/loop9 b 7 9");
			write(out, getFilesDir() + "/busybox losetup /dev/loop9 "+getFilesDir()+"/temproot.ext");
			write(out, getFilesDir() + "/busybox mkfs.ext2 /dev/loop9");
			write(out, getFilesDir() + "/busybox mount -t ext2 /dev/loop9 /system/bin");
			write(out, getFilesDir() + "/busybox cp -rp "+getFilesDir()+"/bin/* /system/bin/");
			write(out, getFilesDir() + "/busybox cp "+getFilesDir()+"/su /system/bin");
			write(out, getFilesDir() + "/busybox cp "+getFilesDir()+"/busybox /system/bin");
			write(out, getFilesDir() + "/busybox chown 0 /system/bin/su");
			write(out, getFilesDir() + "/busybox chown 0 /system/bin/busybox");
			write(out, getFilesDir() + "/busybox chmod 4755 /system/bin/su");
			write(out, getFilesDir() + "/busybox chmod 755 /system/bin/busybox");
			write(out, "pm install "+getFilesDir()+"/SuperUser.apk");
			write(out, "checkvar=checked");
			write(out, "echo finished $checkvar");
		} catch (Exception ex) {
			ex.printStackTrace();
		}
	}

	public void dopermroot() {
		PowerManager pm = (PowerManager) getSystemService(POWER_SERVICE);
		final WakeLock wl = pm.newWakeLock(PowerManager.FULL_WAKE_LOCK | PowerManager.ACQUIRE_CAUSES_WAKEUP | PowerManager.ON_AFTER_RELEASE, "z4root");
		wl.acquire();
		Log.i("AAA", "Starting");

		final int[] processId = new int[1];
		final FileDescriptor fd = Exec.createSubprocess("/system/bin/sh", "-", null, processId);
		Log.i("AAA", "Got processid: " + processId[0]);

		final FileOutputStream out = new FileOutputStream(fd);
		final FileInputStream in = new FileInputStream(fd);


		new Thread() {
			public void run() {
				byte[] mBuffer = new byte[4096];
				int read = 0;
				while (read >= 0) {
					try {
						read = in.read(mBuffer);
						String str = new String(mBuffer, 0, read);
						//saystuff(str);
						Log.i("AAA", str);
					} catch (Exception ex) {
						
					}
				}
				wl.release();
			}
		}.start();

		try {
			String command = "id\n";
			out.write(command.getBytes());
			out.flush();
			try {
				SaveIncludedZippedFileIntoFilesFolder(R.raw.busybox, "busybox", getApplicationContext());
				SaveIncludedZippedFileIntoFilesFolder(R.raw.su, "su", getApplicationContext());
				SaveIncludedZippedFileIntoFilesFolder(R.raw.superuser, "SuperUser.apk", getApplicationContext());
			} catch (Exception e1) {
				e1.printStackTrace();
			}
			command = "chmod 777 " + getFilesDir() + "/busybox\n";
			out.write(command.getBytes());
			out.flush();
			command = getFilesDir() + "/busybox mount -o remount,rw /system\n";
			out.write(command.getBytes());
			out.flush();
			command = getFilesDir() + "/busybox cp "+getFilesDir()+"/su /system/bin/\n";
			out.write(command.getBytes());
			out.flush();
			command = getFilesDir() + "/busybox cp "+getFilesDir()+"/SuperUser.apk /system/app\n";
			out.write(command.getBytes());
			out.flush();
			command = getFilesDir() + "/busybox cp "+getFilesDir()+"/busybox /system/bin/\n";
			out.write(command.getBytes());
			out.flush();
			command = "chown root.root /system/bin/busybox\nchmod 755 /system/bin/busybox\n";
			out.write(command.getBytes());
			out.flush();
			command = "chown root.root /system/bin/su\n";
			out.write(command.getBytes());
			out.flush();
			command = getFilesDir() + "/busybox chmod 6755 /system/bin/su\n";
			out.write(command.getBytes());
			out.flush();
			command = "chown root.root /system/app/SuperUser.apk\nchmod 755 /system/app/SuperUser.apk\n";
			out.write(command.getBytes());
			out.flush();
			command = "rm "+getFilesDir()+"/busybox\n";
			out.write(command.getBytes());
			out.flush();
			command = "rm "+getFilesDir()+"/su\n";
			out.write(command.getBytes());
			out.flush();
			command = "rm "+getFilesDir()+"/SuperUser.apk\n";
			out.write(command.getBytes());
			out.flush();
			command = "rm "+getFilesDir()+"/rageagainstthecage\n";
			out.write(command.getBytes());
			out.flush();
			command = "echo \"reboot now!\"\n";
			saystuff("Rebooting...");
			out.write(command.getBytes());
			out.flush();		
			Thread.sleep(3000);
			command = "sync\nsync\n";
			out.write(command.getBytes());
			out.flush();
			command = "reboot\n";
			out.write(command.getBytes());
			out.flush();
		} catch (Exception ex) {
			ex.printStackTrace();
		}
	}

	public static void SaveIncludedZippedFileIntoFilesFolder(int resourceid, String filename, Context ApplicationContext) throws Exception {
		InputStream is = ApplicationContext.getResources().openRawResource(resourceid);
		FileOutputStream fos = ApplicationContext.openFileOutput(filename, Context.MODE_WORLD_READABLE);
		GZIPInputStream gzis = new GZIPInputStream(is);
		byte[] bytebuf = new byte[1024];
		int read;
		while ((read = gzis.read(bytebuf)) >= 0) {
			fos.write(bytebuf, 0, read);
		}
		gzis.close();
		fos.getChannel().force(true);
		fos.flush();
		fos.close();
	}
	
	public void write(FileOutputStream out, String command) throws IOException {
		command += "\n";
		out.write(command.getBytes());
		out.flush();
	}
}
