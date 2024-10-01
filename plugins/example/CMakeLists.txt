project(Proto VERSION 1.0.0)
set(BaseTargetName example)

juce_add_plugin("${BaseTargetName}"
	# PRODUCT_NAME "Diet Audio"
	# COMPANY_NAME "unplugred"
	# COMPANY_WEBSITE "https://vst.unplug.red"
	# COMPANY_EMAIL "melody@unplug.red"
	FORMATS Standalone VST3
	IS_SYNTH FALSE
	NEEDS_MIDI_INPUT FALSE
	NEEDS_MIDI_OUTPUT FALSE
	IS_MIDI_EFFECT FALSE
	EDITOR_WANTS_KEYBOARD_FOCUS FALSE
	DESCRIPTION "Example Plugin"
	PLUGIN_MANUFACTURER_CODE Soykaf
	PLUGIN_CODE example
	VST3_CATEGORIES Dynamics
	VST2_CATEGORY kPlugCategEffect
	# AU_MAIN_TYPE kAudioUnitType_Effect
	COPY_PLUGIN_AFTER_BUILD TRUE
	VST3_COPY_DIR "${outdir}"
	VST_COPY_DIR "${outdir}"
	# AU_COPY_DIR "${outdir}"
	)

target_compile_features("${BaseTargetName}" PRIVATE cxx_std_17)

target_sources("${BaseTargetName}" PRIVATE
	source/PluginEditor.cpp
	source/PluginProcessor.cpp)

target_compile_definitions("${BaseTargetName}" PUBLIC
	JUCE_DISPLAY_SPLASH_SCREEN=0
	JUCE_USE_FLAC=0
	JUCE_USE_OGGVORBIS=0
	JUCE_USE_MP3AUDIOFORMAT=0
	JUCE_USE_LAME_AUDIO_FORMAT=0
	JUCE_USE_WINDOWS_MEDIA_FORMAT=0
	JUCE_VST3_CAN_REPLACE_VST2=0
	JUCE_USE_CURL=0
	JUCE_LOAD_CURL_SYMBOLS_LAZILY=1
	JUCE_STRICT_REFCOUNTEDPOINTER=1
	JUCE_WEB_BROWSER=0
	JUCE_USE_WIN_WEBVIEW2=0
	JUCE_USE_CAMERA=0)


target_link_libraries("${BaseTargetName}" PRIVATE
	juce::juce_dsp
	juce::juce_audio_utils
	juce::juce_audio_devices
	PUBLIC
	#juce::juce_recommended_warning_flags
	juce::juce_recommended_lto_flags
	juce::juce_recommended_config_flags)


add_custom_command(TARGET ${BaseTargetName} POST_BUILD
				COMMAND ${CMAKE_COMMAND} -E copy_directory
				$<TARGET_FILE_DIR:${BaseTargetName}> "${outdir}")
