package com.z4mod.z4root2;

import java.io.File;

import android.app.Activity;
import android.app.AlertDialog;
import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.util.Log;
import android.view.Menu;
import android.view.MenuInflater;
import android.view.MenuItem;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.TextView;

import com.z4mod.z4root2.VirtualTerminal.VTCommandResult;

public class z4root extends Activity {

	boolean disabled = false;
	Button rootbutton, unrootbutton, temprootbutton;
	TextView detailtext;
	public static final String PREFS_NAME = "z4rootprefs";
	public static final String PREFS_ADS = "AdsEnabled";
	public static final String PREFS_MODE = "rootmode";
	public static final int MODE_PERMROOT = 0;
	public static final int MODE_TEMPROOT = 1;
	public static final int MODE_UNROOT = 2;
	final static String VERSION = "1.3.0";
	boolean forceunroot = true;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

		SharedPreferences settings = getSharedPreferences(PREFS_NAME, 0);
		boolean AdsEnabled = settings.getBoolean(PREFS_ADS, true);
		if (AdsEnabled) {
			setContentView(R.layout.z4rootwadd);
		} else {
			setContentView(R.layout.z4root);
		}

		rootbutton = (Button) findViewById(R.id.rootbutton);
		unrootbutton = (Button) findViewById(R.id.unrootbutton);
		detailtext = (TextView) findViewById(R.id.detailtext);
		temprootbutton = (Button) findViewById(R.id.temprootbutton);

		rootbutton.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {
				if (disabled)
					return;
				disabled = true;
				Intent i = new Intent(z4root.this, Phase1.class);
				SharedPreferences settings = getSharedPreferences(PREFS_NAME, 0);
				SharedPreferences.Editor editor = settings.edit();
				editor.putInt(PREFS_MODE, MODE_PERMROOT);
				editor.commit();
				startActivity(i);
				finish();
			}
		});
		
		temprootbutton.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {
				if (disabled)
					return;
				disabled = true;
				Intent i = new Intent(z4root.this, Phase1.class);
				SharedPreferences settings = getSharedPreferences(PREFS_NAME, 0);
				SharedPreferences.Editor editor = settings.edit();
				editor.putInt(PREFS_MODE, MODE_TEMPROOT);
				editor.commit();
				startActivity(i);
				finish();
			}
		});

		unrootbutton.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {
				if (disabled)
					return;
				disabled = true;
				Intent i;
				if (forceunroot) {
					i = new Intent(z4root.this, Phase1.class);
					SharedPreferences settings = getSharedPreferences(PREFS_NAME, 0);
					SharedPreferences.Editor editor = settings.edit();
					editor.putInt(PREFS_MODE, MODE_UNROOT);
					editor.commit();
				} else {
					i = new Intent(z4root.this, PhaseRemove.class);
				}
				startActivity(i);
				finish();
			}
		});

		new Thread() {
			public void run() {
				dostuff();
			};
		}.start();
	}

	public void dostuff() {
		try {
			final VirtualTerminal vt = new VirtualTerminal();
			VTCommandResult r = vt.runCommand("id");
			if (r.success()) {
				// Rooted device
				forceunroot = false;
				runOnUiThread(new Runnable() {

					@Override
					public void run() {
						unrootbutton.setVisibility(View.VISIBLE);
						temprootbutton.setVisibility(View.GONE);
						rootbutton.setText("Re-root");
						detailtext
								.setText("Your device is already rooted. You can remove the root using the Un-root button, which will delete all of the files installed to root your device. You can also re-root your device if your root is malfunctioning.");
					}
				});
			}
			vt.shutdown();
		} catch (Exception ex) {
			ex.printStackTrace();
		}
		if (forceunroot) {
			Log.i("exists", "/system/bin/su "+new File("/system/bin/su").exists());
			Log.i("exists", "/system/xbin/su "+new File("/system/xbin/su").exists());
			if (new File("/system/bin/su").exists() || new File("/system/xbin/su").exists()) {
				runOnUiThread(new Runnable() {

					@Override
					public void run() {
						unrootbutton.setVisibility(View.VISIBLE);
						detailtext.setText(detailtext.getText() + " Your device already has a 'su' binary, and so the force unroot option has been activated for you.");
					}
				});
			}
		}
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		MenuInflater inflater = getMenuInflater();
		inflater.inflate(R.menu.mainmenu, menu);
		SharedPreferences settings = getSharedPreferences(PREFS_NAME, 0);
		boolean AdsEnabled = settings.getBoolean(PREFS_ADS, true);
		if (!AdsEnabled) {
			menu.getItem(1).setTitle("Enable Ads");
		}
		return true;
	}

	@Override
	public boolean onOptionsItemSelected(MenuItem item) {
		// Handle item selection
		switch (item.getItemId()) {
			case R.id.about :
				AlertDialog.Builder builder = new AlertDialog.Builder(this);
				builder.setMessage("z4root Version " + VERSION + "\nMade by RyanZA").setCancelable(true);
				AlertDialog alert = builder.create();
				alert.setOwnerActivity(this);
				alert.show();
				return true;
			case R.id.disableads :
				SharedPreferences settings = getSharedPreferences(PREFS_NAME, 0);
				boolean AdsEnabled = settings.getBoolean(PREFS_ADS, true);
				SharedPreferences.Editor editor = settings.edit();
				editor.putBoolean(PREFS_ADS, !AdsEnabled);
				editor.commit();

				finish();
				Intent i = new Intent(getApplicationContext(), z4root.class);
				startActivity(i);
				return true;
			default :
				return super.onOptionsItemSelected(item);
		}
	}

}
