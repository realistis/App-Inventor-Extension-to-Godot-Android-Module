# App-Inventor-Extension-to-Godot-Android-Module
This project's goal is to be a Registry and help of AIX to Godot Android Module (AIX2GAM?).

[PR](https://github.com/realistis/App-Inventor-Extension-to-Godot-Android-Module/pulls) this page to add your working Godot Android Module.

### Why?
Developing a game is nice, but delivering a new kind of experience on mobile is better.

Android modules help developing new functions tied to mobile games and apps.

In addition to a free game engine, Godot is a great editor for UI (grid, center, buttons, checkboxes, fonts) and screens (preloaded scenes).

### How?
You need a dex viewer like [JADX](https://github.com/skylot/jadx)

You need some open licensed AIX files or Javascript source (check on community websites, you have lists of them like https://7starmedia.in/extensions-thunkable-makeroid-appybuilder-appinventor/)

AIX is simply a zip file, inside you'll find a classes.jar or classes.dex file.

You need some example, here it comes.

## Compared examples of "vibrate":
https://github.com/AursunPairing/Extensions/blob/master/com.sanderjochems.Vibrate.aix
https://github.com/literaldumb/GodotVibrate/blob/master/vibrate/GodotVibrate.java

Please note, according to https://community.appybuilder.com/t/vibrate-extension/191/2, Vibrate.aix is under CC BY-ND 4.0 license (sanderjochems is the author). We do not publish or make derivative work of the extension here, we only show part of code for educational purpose only.

GodotVibrate is MIT licensed by Kaushik Mazumdar.

Code is valid for the version it was written.

For example, `vibrate(long milliseconds)` is superseeded by `vibrate(VibrationEffect)` since API 26 according to official doc (https://developer.android.com/reference/android/os/Vibrator).

Exposed Google API code is not under someone else's license. Google's code is under Apache 2.0 license (comparison [here](https://choosealicense.com/licenses/)).

AFAIU (but I'm not an expert), you can totally write new code about the same function for a different API (and share it without limitations).

AIX wrapper is also exposed in tutorials on the net. I don't know the license (feel free to add it here).

Best would be to follow Google's license to avoid license conflicts.

To avoid problems, write which API version is targeted.

### Imports
	aix (2018) -> classes.jar > jadx
		import android.content.Context;
		import android.os.Vibrator;
		import android.util.Log;
	GodotVibrate (2016)
		import android.app.Activity;
		import android.content.Intent;
		import android.os.Vibrator;
		import android.content.Context;

### Class
	aix
		public class Vibrate extends AndroidNonvisibleComponent implements Component {
	GodotVibrate
		public class GodotVibrate extends Godot.SingletonBase {
		Activity m_pActivity;
		
### Function
	aix
		public void Vibrate(long miliseconds)
	GodotVibrate
		public void doVibrate(int ms)

### Function Content	
	aix
		Vibrator vibrator = (Vibrator) this.context.getSystemService("vibrator");
				if (vibrator.hasVibrator()) {
					vibrator.vibrate(miliseconds);
	GodotVibrate
			Vibrator v = (Vibrator) m_pActivity.getSystemService(Context.VIBRATOR_SERVICE);
			
			// Vibrate for ms milliseconds
			v.vibrate(ms);

### Godot needs to initialize
		static public Godot.SingletonBase initialize(Activity p_activity) {
			return new GodotVibrate(p_activity);


### Godot needs a constructor
	public GodotVibrate(Activity p_activity) {
		 m_pActivity = p_activity;
		//register class name and functions to bind
		registerClass("GodotVibrate", new String[]{"doVibrate"});

Also visible at https://github.com/kloder-games/godot-admob/blob/master/admob/android/GodotAdMob.java

### Godot also need a config.py
	def can_build(platform):
		return platform=="android"

	def configure(env):
		if env['platform'] == 'android':
			# android specific code
		env.android_add_java_dir("android")
		env.disable_module()

### Godot needs a SCsub file
	Import('env')

## Compilations
Now you've written your great module, it's time to compile it.
Frist, please read the doc about it at https://docs.godotengine.org/en/latest/development/compiling/compiling_for_android.html#doc-compiling-for-android

Easiest wat to get the right source with the right Godot editor (mandatory) is to download the latest Godot editor + templates from, for example, https://downloads.tuxfamily.org/godotengine/3.1/beta3/ set the branch at the right commit by reading the [README file](https://downloads.tuxfamily.org/godotengine/3.1/beta3/README.txt) for the built editor and modify the URL like https://github.com/godotengine/godot/tree/a8510331c0115eeee2d6ac0a4acbeb5d4df833b3, then clone it at this point (Github Desktop is great for that).

For release versions, it's easier, simply download source from https://github.com/godotengine/godot/releases and built editor and templates from the provided link.

No you have Godot source, built templates and editor right.

### Compile with minimal settings
	scons -j4 android_arch=armv7 platform=android target=release_debug optimize=speed tools=no use_lto=no deprecated=no gdscript=no minizip=no xaudio2=no disable_3d=yes disable_advanced_gui=yes no_editor_splash=yes builtin_bullet=no builtin_certs=no builtin_enet=no builtin_libogg=no builtin_libtheora=no builtin_libvorbis=no builtin_libvpx=no builtin_libwebp=no builtin_libwebsockets=no builtin_mbedtls=no builtin_miniupnpc=no builtin_opus=no builtin_pcre2=no builtin_recast=no builtin_squish=no builtin_thekla_atlas=no builtin_xatlas=no builtin_zlib=no debug_symbols=no separate_debug_symbols=no android_neon=no android_stl=no module_bmp_enabled=no module_bullet_enabled=no module_csg_enabled=no module_cvtt_enabled=no module_dds_enabled=no module_enet_enabled=no module_etc_enabled=no module_gdnative_enabled=no module_gdscript_enabled=no module_gridmap_enabled=no module_hdr_enabled=no module_jpg_enabled=no module_mbedtls_enabled=no module_mobile_vr_enabled=no module_mono_enabled=no module_ogg_enabled=no module_opensimplex_enabled=no module_opus_enabled=no module_pvr_enabled=no module_recast_enabled=no module_regex_enabled=no module_squish_enabled=no module_stb_vorbis_enabled=no module_svg_enabled=no module_tga_enabled=no module_thekla_unwrap_enabled=no module_theora_enabled=no module_tinyexr_enabled=no module_upnp_enabled=no module_visual_script_enabled=no module_vorbis_enabled=no module_webm_enabled=no module_webp_enabled=no module_websocket_enabled=no module_xatlas_unwrap_enabled=no config=force

If it compiles (scons+gradlew), Add your module to /modules

### Compile minimal+module (your module is added by default)
	scons -j4 android_arch=armv7 platform=android target=release_debug optimize=speed tools=no use_lto=no deprecated=no gdscript=no minizip=no xaudio2=no disable_3d=yes disable_advanced_gui=yes no_editor_splash=yes builtin_bullet=no builtin_certs=no builtin_enet=no builtin_libogg=no builtin_libtheora=no builtin_libvorbis=no builtin_libvpx=no builtin_libwebp=no builtin_libwebsockets=no builtin_mbedtls=no builtin_miniupnpc=no builtin_opus=no builtin_pcre2=no builtin_recast=no builtin_squish=no builtin_thekla_atlas=no builtin_xatlas=no builtin_zlib=no debug_symbols=no separate_debug_symbols=no android_neon=no android_stl=no module_bmp_enabled=no module_bullet_enabled=no module_csg_enabled=no module_cvtt_enabled=no module_dds_enabled=no module_enet_enabled=no module_etc_enabled=no module_gdnative_enabled=no module_gdscript_enabled=no module_gridmap_enabled=no module_hdr_enabled=no module_jpg_enabled=no module_mbedtls_enabled=no module_mobile_vr_enabled=no module_mono_enabled=no module_ogg_enabled=no module_opensimplex_enabled=no module_opus_enabled=no module_pvr_enabled=no module_recast_enabled=no module_regex_enabled=no module_squish_enabled=no module_stb_vorbis_enabled=no module_svg_enabled=no module_tga_enabled=no module_thekla_unwrap_enabled=no module_theora_enabled=no module_tinyexr_enabled=no module_upnp_enabled=no module_visual_script_enabled=no module_vorbis_enabled=no module_webm_enabled=no module_webp_enabled=no module_websocket_enabled=no module_xatlas_unwrap_enabled=no config=force

Godot modules, some come with implementation, some not.

At gradlew step, If you get something like:

`Program type already present: android.support.v4.app`

you need to REMOVE the implementation line from `platform/android/build.gradle.template`, remove:

`implementation "com.android.support:support-core-utils:28.0.0"`

If you get:

`package android.support.v4.app does not exist`

re-write the implementation line

If it compiles, you can celebrate and keep this one to test compatibility with other Godot libraries and modules.

You can compile a new default version with other godot libraries.

Add all modules you want to project.godot (previously engine.cfg)

`    [android]
    modules="org/godotengine/godot/GodotVibrate","org/godotengine/godot/GodotAdMob"
Use in gdscript
    var singleton = Globals.get_singleton("GodotVibrate")
    singleton.doVibrate(200) # milliseconds for API<26`
	
`Project settings > Application → Run → Low Processor Mode` (repaint only when needed) for non-game apps, but only for desktop apps for now. This will be a game changer once this function will work for mobiles!

Export using the compiled template
	`Export->Target->Android`
Custom Package (Debug/Release):
	Point to the newly built apk
Permissions: 
	`Vibrate` (for the vibration module)

Test your module

# Congrats!
Now you're module is working, you can make it a project for others to compile with their modules and proudly add it here by making a [PR](https://github.com/realistis/App-Inventor-Extension-to-Godot-Android-Module/pulls):
