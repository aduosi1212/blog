---
layout: post
title:  "虚幻4插件语言UPL，用于编辑修改XML文件，比如Android的AndroidManifest"
date:   2017-05-23 10:05:00 +0800
categories: "ue4"
tag: ["android", "sdk"]
---
* content
{:toc}

 UnrealPluginLanguage (UPL) is a simple XML-based language for manipulating XML and returning
	 * strings.  It contains an <init> section which is evaluated once per architecture before any
	 * other sections.  The state is maintained and carried forward to the next section evaluated
	 * so the order the sections are executed matters.
	 *
	 * While UPL is a general system for modifying and quering XML it is specifically used to allow
	 * plug-ins to effect the global configuration of the package that they are a part of. For
	 * example, this allows a plug-in to modify an Android's APK AndroidManfiest.xml file or an
	 * IOS IPA's plist file. UBT will also query a plug-in's UPL xml file for strings to be included
	 * in files that must be common to the package such as some .java files on Android.
	 *
	 * If you need to see the instructions executed in your plugin context add the following to
	 * enable tracing:
	 *
	 *	<trace enable="true"/>
	 *
	 * After this instuction all the nodes actually executed in your context will be written to the
	 * log until you do a <trace enable="false"/>.  You can also get a dump of all the variables in
	 * your context with this command:
	 *
	 *	<dumpvars/>
	 *
	 * Bool, Int, and String variable types are supported.  Any attribute may reference a variable
	 * and will be replaced with the string equivalent before evaluation using this syntax:
	 *
	 *	$B(name) = boolean variable "name"'s value
	 *	$I(name) = integer variable "name"'s value
	 *	$S(name) = string variable "name"'s value
	 *	$E(name) = element variable "name"'s value
	 *
	 * The following variables are initialized automatically:
	 *
	 *	$S(Output) = the output returned for evaluating the section (initialized to Input)
	 *	$S(Architecture) = target architecture (armeabi-armv7a, arm64-v8a, x86, x86_64)
	 *	$S(PluginDir) = directory the XML file was loaded from
	 *	$S(EngineDir) = engine directory
	 *	$S(BuildDir) = project's platform appropriate build directory (within the Intermediate folder)
	 *	$B(Distribution) = true if distribution build
	 *
	 * Note: with the exception of the above variables, all are in the context of the plugin to
	 * prevent namespace collision; trying to set a new value to any of the above, with the
	 * exception of Output, will only affect the current context.
	 *
	 * The following nodes allow manipulation of variables:
	 *
	 *	<setBool result="" value=""/>
	 *	<setInt result="" value=""/>
	 *	<setString result="" value=""/>
	 *	<setElement result="" value=""/>
	 *	<setElement result="" value="" text=""/>
	 *	<setElement result="" xml=""/>
	 *
	 * <setElement> with value creates an empty XML element with the tag set to value.
	 * <setElement> with value and text creates an XML element with tag set to value of unparsed text.
	 * <setElement> with xml will parse the XML provided.  Remember to escape any special characters!
	 *
	 * Variables may also be set from a property in an ini file:
	 *
	 *	<setBoolFromProperty result="" ini="" section="" property="" default=""/>
	 *	<setIntFromProperty result="" ini="" section="" property="" default=""/>
	 *	<setStringFromProperty result="" ini="" section="" property="" default=""/>
	 *
	 * Boolean variables may also be set to the result of applying operators:
	 *
	 *	<setBoolNot result="" source=""/>
	 *	<setBoolAnd result="" arg1="" arg2=""/>
	 *	<setBoolOr result="" arg1="" arg2=""/>
	 *	<setBoolIsEqual result="" arg1="" arg2=""/>
	 *	<setBoolIsLess result="" arg1="" arg2=""/>
	 *	<setBoolIsLessEqual result="" arg1="" arg2=""/>
	 *	<setBoolIsGreater result="" arg1="" arg2=""/>
	 *	<setBoolIsGreaterEqual result="" arg1="" arg2=""/>
	 *
	 * Integer variables may use these arithmetic operations:
	 *
	 *	<setIntAdd result="" arg1="" arg2=""/>
	 *	<setIntSubtract result="" arg1="" arg2=""/>
	 *	<setIntMultiply result="" arg1="" arg2=""/>
	 *	<setIntDivide result="" arg1="" arg2=""/>
	 *
	 * Strings are manipulated with the following:
	 *
 	 *	<setStringAdd result="" arg1="" arg2=""/>
	 *	<setStringSubstring result="" source="" start="" length=""/>
	 *	<setStringReplace result="" source="" find="" with=""/>
	 *
	 * String length may be retrieved with:
	 *
	 *	<setIntLength result="" source=""/>
	 *
	 * The index of a search string may be found in source with:
	 *
	 *	<setIntFindString result="" source="" find=""/>
	 *
	 * The following shortcut string comparisons may also be used instead of using <setIntFindString>
	 * and checking the result:
	 *
	 *	<setBoolStartsWith result="" source="" find=""/>
	 *	<setBoolEndsWith result="" source="" find=""/>
	 *	<setBoolContains result="" source="" find=""/>
	 *
	 * Messages are written to the log with this node:
	 *
	 *	<log text=""/>
	 *
	 * Conditional execution uses the following form:
	 *
	 *	<if condition="">
	 *		<true>
	 *			<!-- executes if boolean variable in condition is true -->
	 *		</true>
	 *		<false>
	 *			<!-- executes if boolean variable in condition is false -->
	 *		</false>
	 *	</if>
	 *
	 * The <true> and <false> blocks are optional.  The condition must be in a boolean variable.
	 * The boolean operator nodes may be combined to create a final state for more complex
	 * conditions:
	 *
	 *	<setBoolNot result="notDistribution" value="$B(Distribution)/>
	 *	<setBoolEquals result="isX86" arg1="$S(Architecture)" arg2="x86"/>
	 *	<setBoolEquals result="isX86_64" arg2="$S(Architecture)" arg2="x86_64/">
	 *	<setBoolOr result="isIntel" arg1="$B(isX86)" arg2="$B(isX86_64)"/>
	 *	<setBoolAnd result="intelAndNotDistribution" arg1="$B(isIntel)" arg2="$B(notDistribution)"/>
	 *	<if condition="intelAndNotDistribution">
	 *		<true>
	 *			<!-- do something for Intel if not a distribution build -->
	 *		</true>
	 *	</if>
	 *
	 * Note the "isIntel" could also be done like this:
	 *
	 *	<setStringSubstring result="subarch" source="$S(Architecture)" start="0" length="3"/>
	 *	<setBoolEquals result="isIntel" arg1="$S(subarch)" arg2="x86"/>
	 *
	 * Two shortcut nodes are available for conditional execution:
	 *
	 *	<isArch arch="armeabi-armv7">
	 *		<!-- do stuff -->
	 *	</isArch>
	 *
	 * is the equivalent of:
	 *
	 *	<setBoolEquals result="temp" arg1="$S(Architecture)" arg2="armeabi-armv7">
	 *	<if condition="temp">
	 *		<true>
	 *			<!-- do stuff -->
	 *		</true>
	 *	</if>
	 *
	 * and
	 *
	 *	<isDistribution>
	 *		<!-- do stuff -->
	 *	</isDistribution>
	 *
	 * is the equivalent of:
	 *
	 *	<if condition="$B(Distribution)">
	 *		<!-- do stuff -->
	 *	</if>
	 *
	 * Execution may be stopped with:
	 *
	 *	<return/>
	 *	 
	 * Loops may be created using these nodes:
	 *
	 *	<while condition="">
	 *		<!-- do stuff -->
	 *	</while>
	 *
	 *	<break/>
	 *	<continue/>
	 *
	 * The <while> body will execute until the condition is false or a <break/> is hit.  The
	 * <continue/> will restart execution of the loop if the condition is still true or exit.
	 *
	 * Note: <break/> outside a <while> body will act the same as <return/>
	 *
	 * Here is an example loop which writes 1 to 5 to the log, skipping 3.  Note the update of the
	 * while condition should be done before the continue otherwise it may not exit.
	 *
	 *	<setInt result="index" value="0"/>
	 *	<setBool result="loopRun" value="true"/>
	 *	<while condition="loopRun">
	 *		<setIntAdd result="index" arg1="$I(index)" arg2="1"/>
	 *		<setBoolIsLess result="loopRun" arg1="$I(index)" arg2="5"/>
	 *		<setBoolIsEqual result="indexIs3" arg1="$I(index)" arg2="3"/>
	 *		<if condition="indexIs3">
	 *			<true>
	 *				<continue/>
	 *			</true>
	 *		</if>
	 *		<log text="$I(index)"/>
	 *	</while>
	 *
	 * It is possible to use variable replacement in generating the result variable
	 * name as well.  This makes the creation of arrays in loops possible:
	 *
	 *	<setString result="array_$I(index)" value="element $I(index) in array"/>
	 *
	 * This may be retrieved using the following (value is treated as the variable
	 * name):
	 *
	 *	<setStringFrom result="out" value="array_$I(index)"/>
	 *
	 * For boolean and integer types, you may use <setBoolFrom/> and <setIntFrom/>.
	 *
	 * Nodes for inserting text into the section are as follows:
	 *
	 *	<insert> body </insert>
	 *	<insertNewline/>
	 *	<insertValue value=""/>
	 *	<loadLibrary name="" failmsg=""/>
	 *
	 * The first one will insert either text or nodes into the returned section
	 * string.  Please note you must use escaped characters for:
	 *
	 *	< = &lt;
	 *	> = &gt;
	 *	& = &amp;
	 *
	 *	<insertValue value=""/> evaluates variables in value before insertion.  If value contains
	 * double quote ("), you must escape it with &quot;.
	 *
	 *	<loadLibrary name="" failmsg=""/> is a shortcut to insert a system.LoadLibrary try/catch
	 * block with an optional logged message for failure to load case.
	 *
	 * You can do a search and replace in the Output with:
	 *
	 *	<replace find="" with=""/>
	 *
	 * Note you can also manipulate the actual $S(Output) directly, the above are more efficient:
	 *
	 *	<setStringAdd result="Input" arg1="$S(Output)" arg2="sample\n"/>
	 *	<setStringReplace result="Input" source="$S(Output)" find=".LAUNCH" with=".INFO"/>
	 *
	 * XML manipulation uses the following nodes:
	 *
	 *	<addElement tag="" name=""/>
	 *	<addElements tag=""> body </addElements>
	 *	<removeElement tag=""/>
	 *	<setStringFromTag result="" tag="" name=""/>
	 *	<setStringFromAttribute result="" tag="" name=""/>
	 *	<addAttribute tag="" name="" value=""/>
	 *	<removeAttribute tag="" name=""/>
	 *	<loopElements tag=""> instructions </loopElements>
	 *
	 * The current element is referenced with tag="$".  Element variables are referenced with $varname
	 * since using $E(varname) will be expanded to the string equivalent of the XML.
	 *
	 * <uses-permission>, <uses-feature>, and <uses-library> are updated with:
	 *
	 *	<addPermission android:name="" .. />
	 *	<addFeature android:name="" .. />
	 *	<addLibrary android:name="" .. />
	 *
	 * Any attributes in the above commands are copied to the element added to the manifest so you
	 * can do the following, for example:
	 *
	 *	<addFeature android:name="android.hardware.usb.host" android:required="true"/>
	 *
	 * Finally, these nodes allow copying of files useful for staging jar and so files:
	 *
	 *	<copyFile src="" dst=""/>
	 *	<copyDir src="" dst=""/>
	 *
	 * The following should be used as the base for the src and dst paths:
	 *
	 *	$S(PluginDir) = directory the XML file was loaded from
 	 *	$S(EngineDir) = engine directory
	 *	$S(BuildDir) = project's platform appropriate build directory
	 *
	 * While it is possible to write outside the APK directory, it is not recommended.
	 *
	 * If you must remove files (like development-only files from distribution builds) you can
	 * use this node:
	 *
	 *	<deleteFiles filespec=""/>
	 *
	 * It is restricted to only removing files from the BuildDir.  Here is example usage to remove
	 * the Oculus Signature Files (osig) from the assets directory:
	 *
	 *	<deleteFiles filespec="assets/oculussig_*"/>
	 *
	 * The following sections are evaluated during the packaging or deploy stages:
	 *
	 * ** For all platforms **
	 *	<!-- init section is always evaluated once per architecture -->
	 * 	<init> </init>
	 *
	 * ** Android Specific sections **
	 * 	<!-- optional updates applied to AndroidManifest.xml -->
	 * 	<androidManifestUpdates> </androidManifestUpdates>
	 * 	
	 * 	<!-- optional additions to proguard -->
	 * 	<proguardAdditions>	</proguardAdditions>
	 * 	
	 * 	<!-- optional additions to generated build.xml before ${sdk.dir}/tools/ant/build.xml import -->
	 * 	<buildXmlPropertyAdditions> </buildXmlPropertyAdditions>
	 *
	 * 	<!-- optional files or directories to copy or delete from Intermediate/Android/APK before ndk-build -->
	 * 	<prebuildCopies> </prebuildCopies>
	 * 	
	 * 	<!-- optional files or directories to copy or delete from Intermediate/Android/APK after ndk-build -->
	 * 	<resourceCopies> </resourceCopies>
	 * 	
	 * 	<!-- optional additions to the GameActivity imports in GameActivity.java -->
	 * 	<gameActivityImportAdditions> </gameActivityImportAdditions>
	 *
	 * 	<!-- optional additions to the GameActivity after imports in GameActivity.java -->
	 *  <gameActivityPostImportAdditions> </gameActivityPostImportAdditions>
	 *  
	 * 	<!-- optional additions to the GameActivity class in GameActivity.java -->
	 * 	<gameActivityClassAdditions> </gameActivityOnClassAdditions>
	 * 	
	 * 	<!-- optional additions to GameActivity onCreate metadata reading in GameActivity.java -->
	 * 	<gameActivityReadMetadata> </gameActivityReadMetadata>
	 *
	 *	<!-- optional additions to GameActivity onCreate in GameActivity.java -->
	 *	<gameActivityOnCreateAdditions> </gameActivityOnCreateAdditions>
	 * 	
	 * 	<!-- optional additions to GameActivity onDestroy in GameActivity.java -->
	 * 	<gameActivityOnDestroyAdditions> </gameActivityOnDestroyAdditions>
	 * 	
	 * 	<!-- optional additions to GameActivity onStart in GameActivity.java -->
	 * 	<gameActivityOnStartAdditions> </gameActivityOnStartAdditions>
	 * 	
	 * 	<!-- optional additions to GameActivity onStop in GameActivity.java -->
	 * 	<gameActivityOnStopAdditions> </gameActivityOnStopAdditions>
	 * 	
	 * 	<!-- optional additions to GameActivity onPause in GameActivity.java -->
	 * 	<gameActivityOnPauseAdditions> </gameActivityOnPauseAdditions>
	 * 	
	 * 	<!-- optional additions to GameActivity onResume in GameActivity.java -->
	 *	<gameActivityOnResumeAdditions>	</gameActivityOnResumeAdditions>
	 *
	 * 	<!-- optional additions to GameActivity onActivityResult in GameActivity.java -->
	 * 	<gameActivityOnActivityResultAdditions>	</gameActivityOnActivityResultAdditions>
	 * 	
	 * 	<!-- optional libraries to load in GameActivity.java before libUE4.so -->
	 * 	<soLoadLibrary>	</soLoadLibrary>
	 * 	
	 *
	 * Here is the complete list of supported nodes:
	 *
	 * <isArch arch="">
	 * <isDistribution>
	 * <if> => <true> / <false>
	 * <while condition="">
	 * <return/>
	 * <break/>
	 * <continue/>
	 * <log text=""/>
	 * <insert> </insert>
	 * <insertValue value=""/>
	 * <replace find="" with""/>
	 * <copyFile src="" dst=""/>
	 * <copyDir src="" dst=""/>
	 * <loadLibrary name="" failmsg=""/>
	 * <setBool result="" value=""/>
	 * <setBoolFrom result="" value=""/>
	 * <setBoolFromProperty result="" ini="" section="" property="" default=""/>
	 * <setBoolNot result="" source=""/>
	 * <setBoolAnd result="" arg1="" arg2=""/>
	 * <setBoolOr result="" arg1="" arg2=""/>
	 * <setBoolIsEqual result="" arg1="" arg2=""/>
	 * <setBoolIsLess result="" arg1="" arg2=""/>
	 * <setBoolIsLessEqual result="" arg1="" arg2=""/>
	 * <setBoolIsGreater result="" arg1="" arg2=""/>
	 * <setBoolIsGreaterEqual result="" arg1="" arg2=""/>
	 * <setInt result="" value=""/>
	 * <setIntFrom result="" value=""/>
	 * <setIntFromProperty result="" ini="" section="" property="" default=""/>
	 * <setIntAdd result="" arg1="" arg2=""/>
	 * <setIntSubtract result="" arg1="" arg2=""/>
	 * <setIntMultiply result="" arg1="" arg2=""/>
	 * <setIntDivide result="" arg1="" arg2=""/>
	 * <setIntLength result="" source=""/>
	 * <setIntFindString result="" source="" find=""/>
	 * <setString result="" value=""/>
	 * <setStringFrom result="" value=""/>
	 * <setStringFromProperty result="" ini="" section="" property="" default=""/>
	 * <setStringAdd result="" arg1="" arg2=""/>
	 * <setStringSubstring result="" source="" index="" length=""/>
	 * <setStringReplace result="" source="" find="" with=""/>
