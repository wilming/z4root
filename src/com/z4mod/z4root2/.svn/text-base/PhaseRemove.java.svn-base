package com.z4mod.z4root2;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.ProgressBar;
import android.widget.TextView;

public class PhaseRemove extends Activity {

	ProgressBar progressBar;
	TextView infotext;
	
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.pundo);
		
		infotext = (TextView) findViewById(R.id.infotv);
		progressBar = (ProgressBar) findViewById(R.id.spinner);
		
		new Thread() {
			public void run() {
				dostuff();
			};
		}.start();
	}
	
	public void saystuff(final String stuff) {
		runOnUiThread(new Runnable() {

			@Override
			public void run() {
				progressBar.setVisibility(View.GONE);
				infotext.setVisibility(View.VISIBLE);
				infotext.setText(stuff);
			}
		});
	}
	
	public void dostuff() {
		try {
			VirtualTerminal vt = new VirtualTerminal();
			vt.runCommand("busybox mount -o remount,rw /system");
			vt.runCommand("rm /system/bin/busybox");
			vt.runCommand("rm /system/bin/su");
			vt.runCommand("rm /system/xbin/busybox");
			vt.runCommand("rm /system/xbin/su");
			vt.runCommand("rm /system/app/SuperUser.apk");
			saystuff("Completed. Your root should now be completely removed.");
		} catch (Exception ex) {
			ex.printStackTrace();
			saystuff("Encountered an error attempting to unroot you!\n\n\n"+ex.getLocalizedMessage());
		}
		
	}
	
}
