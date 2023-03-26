Hi! mungkin kamu baru disini dan ingin membuat servermu optimal dengan uptime diatas 24 jam, dan juga performa gacor anti crash, berarti guide ini cocok buat mu!

## Kenapa [Aikar flags](https://aikar.co/2018/07/02/tuning-the-jvm-g1gc-garbage-collector-flags-for-minecraft/)?
gw mempelajari Aikar flag JVM selama beberapa minggu sebelum bikin ini.  G1 garbage collector menawarkan stabilitas yang hebat dengan kinerja yang mantap, tapi mungkin lambat dalam kriteria tertentu, itu membantu server besar pada waktu itu dan masih membantu mereka saat ini, tetapi Java berevolusi. Semakin banyak garbage collector yang dibuat, dan alternatif yang baik saat ini adalah ShenandoahGC, yang secara drastis meningkatkan penggunaan CPU. Dalam waktu dekat, ZGC akan menjadi referensi garbage collector untuk server Minecraft, tetapi masih dalam pengembangan dan benar-benar mematikan CPU Anda. Itu sebabnya gw tetap menggunakan G1GC (G1 garbage collector).

## Disclaimer
Pertama dan terpenting, saya tidak bertanggung jawab atas masalah apa pun di server Anda sebagai akibat dari perubahan ini. Ini masih percobaan dan bug bisa terjadi. Tidak ada yang memiliki konfigurasi yang sama, semuanya bisa berbeda dari orang ke orang.

## Penggunaan Distribusi Java
Gw merekomendasi untuk menggunakan [Adoptium](https://adoptium.net/), tidak ada memory leak yang diketahui dan sangat sering diguanakn. Untuk sekarang, gunakan Java 18 karena ada G1GC improvement daripada 17.

## Flags
**Dari 1.18, Java 17 adalah persyaratan minimum. Jika anda menggunakan versi yang lebih jadul , gw sarankan menggunakan Java 11. Jangan pernah menggunakan Java 8, ini adalah versi dari 7 tahun yang lalu dan kinerjanya buruk. Versi baru akan lebih baik dalam kinerja performa.**

***Server Software yang didukung:***
- [x] Vanilla
- [x] Spigot/Paper dan segala jenis Fork-nya Purpur, Pufferfish, dsb – *([Mirai](https://github.com/etil2jz/Mirai) direkomendasi untuk performa dashyat di kasur eh di di minecraft)*
- [x] Sponge
- [x] Fabric
- [x] Quilt
- [x] Forge

***Untuk server yang menggunakan ram 12GB kebawah:***
```java
java -Xms12G -Xmx12G -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1 -XX:-UseBiasedLocking -XX:UseAVX=3 -XX:+UseStringDeduplication -XX:+UseFastUnorderedTimeStamps -XX:+UseAES -XX:+UseAESIntrinsics -XX:UseSSE=4 -XX:+UseFMA -XX:AllocatePrefetchStyle=1 -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+TrustFinalNonStaticFields -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:+UseNUMA -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector -jar server.jar nogui
```

***Untuk server yang menggunakan ram 12GB keatas:***
```java
java -Xms20G -Xmx20G -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=40 -XX:G1MaxNewSizePercent=50 -XX:G1HeapRegionSize=16M -XX:G1ReservePercent=15 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=20 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1 -XX:-UseBiasedLocking -XX:UseAVX=3 -XX:+UseStringDeduplication -XX:+UseFastUnorderedTimeStamps -XX:+UseAES -XX:+UseAESIntrinsics -XX:UseSSE=4 -XX:+UseFMA -XX:AllocatePrefetchStyle=1 -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+TrustFinalNonStaticFields -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:+UseNUMA -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector -jar server.jar nogui
```

*tentu saja, sesuaikan nilai -Xms dan -Xmx dengan ram atau keinginan anda, sama untuk java path dan server jar Anda*

Anda tidak boleh menggunakan lebih dari 20GB RAM, G1GC akan mulai kesulitan dan mulai bermasalah…

## Sources
[Tuning the JVM – G1GC Garbage Collector Flags for Minecraft](https://aikar.co/mcflags.html)

[GraalVM release notes](https://www.graalvm.org/release-notes/)

[java -XX:+PrintFlagsFinal]()

[etil2jz's Guided](https://github.com/etil2jz/etil-minecraft-flags)
