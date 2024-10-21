---
layout: post
title:  "Java apps fonts too big"
author: tap
categories: [ Logs, GUI ]
tags: [ Java, ElementaryOS, SweetHome3D ]
image: assets/images/javafonts-big.webp
---
This was happened when I first tried to run SweetHome3D Portable. As it was a portable software, I didn't want to be bothered by installing it. The apps was running, but as you can see at the picture above, the fonts were way too big on my screen.

The original bash script (located in 'SweetHome3D-7.5-portable/SweetHome3D-linux-x64') for running the app was:

```sh
# Run Sweet Home 3D
exec "$PROGRAM_DIR"/runtime/linux/x64/runtime/bin/java \
-Xmx1024m \
-classpath "$PROGRAM_DIR"/lib/SweetHome3D.jar: \
"$PROGRAM_DIR"/lib/Furniture.jar: \
"$PROGRAM_DIR"/lib/Textures.jar: \
"$PROGRAM_DIR"/lib/Examples.jar: \
"$PROGRAM_DIR"/lib/Help.jar: \
"$PROGRAM_DIR"/lib/batik-svgpathparser-1.7.jar: \
"$PROGRAM_DIR"/lib/jeksparser-calculator.jar: \
"$PROGRAM_DIR"/lib/iText-2.1.7.jar: \
"$PROGRAM_DIR"/lib/freehep-vectorgraphics-svg-2.1.1c.jar: \
"$PROGRAM_DIR"/lib/sunflow-0.07.3i.jar: \
"$PROGRAM_DIR"/lib/jmf.jar: \
"$PROGRAM_DIR"/lib/java3d-1.6/j3dcore.jar: \
"$PROGRAM_DIR"/lib/java3d-1.6/j3dutils.jar: \
"$PROGRAM_DIR"/lib/java3d-1.6/vecmath.jar: \
"$PROGRAM_DIR"/lib/java3d-1.6/gluegen-rt.jar: \
"$PROGRAM_DIR"/lib/java3d-1.6/jogl-java3d.jar: \
"$PROGRAM_DIR"/lib/jnlp.jar \
-Djava.library.path="$PROGRAM_DIR"/lib/java3d-1.6/linux/amd64: \
"$PROGRAM_DIR"/lib/yafaray/linux/x64 \
-Djogamp.gluegen.UseTempJarCache=false \
-Dcom.eteks.sweethome3d.preferencesFolder="$PROGRAM_DIR"/data \
-Dcom.eteks.sweethome3d.applicationFolders="$PROGRAM_DIR"/data \
-Dcom.eteks.sweethome3d.deploymentInformation=portable \
-Dcom.eteks.sweethome3d.applicationId=SweetHome3D#Portable com.eteks.sweethome3d.SweetHome3D \
-open "$1"
```

Add these line somewhere ini the parameter part:

```
-Dcom.eteks.sweethome3d.resolutionScale=1 
-Dswing.defaultlaf=javax.swing.plaf.metal.MetalLookAndFeel 
-Dswing.plaf.metal.controlFont=Dialog-12 
-Dswing.plaf.metal.userFont=SansSerif-12 
-Dswing.plaf.metal.systemFont=SansSerif-12 
```
The code was to tell the program what the resolution, theme and fonts to use.
So it'll become:

```sh
# Run Sweet Home 3D
exec "$PROGRAM_DIR"/runtime/linux/x64/runtime/bin/java \
-Xmx1024m \
-classpath "$PROGRAM_DIR"/lib/SweetHome3D.jar: \
"$PROGRAM_DIR"/lib/Furniture.jar: \
"$PROGRAM_DIR"/lib/Textures.jar: \
"$PROGRAM_DIR"/lib/Examples.jar: \
"$PROGRAM_DIR"/lib/Help.jar: \
"$PROGRAM_DIR"/lib/batik-svgpathparser-1.7.jar: \
"$PROGRAM_DIR"/lib/jeksparser-calculator.jar: \
"$PROGRAM_DIR"/lib/iText-2.1.7.jar: \
"$PROGRAM_DIR"/lib/freehep-vectorgraphics-svg-2.1.1c.jar: \
"$PROGRAM_DIR"/lib/sunflow-0.07.3i.jar: \
"$PROGRAM_DIR"/lib/jmf.jar: \
"$PROGRAM_DIR"/lib/java3d-1.6/j3dcore.jar: \
"$PROGRAM_DIR"/lib/java3d-1.6/j3dutils.jar: \
"$PROGRAM_DIR"/lib/java3d-1.6/vecmath.jar: \
"$PROGRAM_DIR"/lib/java3d-1.6/gluegen-rt.jar: \
"$PROGRAM_DIR"/lib/java3d-1.6/jogl-java3d.jar: \
"$PROGRAM_DIR"/lib/jnlp.jar \
-Djava.library.path="$PROGRAM_DIR"/lib/java3d-1.6/linux/amd64: \
"$PROGRAM_DIR"/lib/yafaray/linux/x64 \
-Djogamp.gluegen.UseTempJarCache=false \ 
-Dcom.eteks.sweethome3d.preferencesFolder="$PROGRAM_DIR"/data \
-Dcom.eteks.sweethome3d.applicationFolders="$PROGRAM_DIR"/data \
-Dcom.eteks.sweethome3d.deploymentInformation=portable \
-Dcom.eteks.sweethome3d.resolutionScale=1 \
-Dswing.defaultlaf=javax.swing.plaf.metal.MetalLookAndFeel \
-Dswing.plaf.metal.controlFont=Dialog-12 \
-Dswing.plaf.metal.userFont=SansSerif-12 \
-Dswing.plaf.metal.systemFont=SansSerif-12 \ 
-Dcom.eteks.sweethome3d.applicationId=SweetHome3D#Portable com.eteks.sweethome3d.SweetHome3D \
-open "$1"
```
And voila! Here's the result:

![Result](/assets/images/javafonts-normal.webp)