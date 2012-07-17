# FakeTime 
(This fork updates instructions for mac, look below for instructions from [dileepbapat])

# FOR Mac:

		###(if not cloned yet)
	~ git clone git://github.com/dileepbapat/faketime.git

	~ cd faketime

		###(replace with the correct location for your jdk, sudo if required)
	~ gcc -shared -I /Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home/include -I /Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home/include/darwin/ -m64 jvmfaketime.c  -o libjvmfaketime.dylib
	~ cp libjvmfaketime.dylib /Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home/jre/lib/

		###(This will update System.class and will create a dir named 'java' in current wd)
	~ cp ~/.m2/repository/javassist/javassist/3.4.GA/javassist-3.4.GA.jar javassist.jar
	~ javac -classpath .:javassist.jar ClassModifier.java 
	~ java -cp .:javassist.jar  ClassModifier

		###(backup and update rt.jar, sudo if required)
	~ cp /Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home/jre/lib/rt.jar /Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home/jre/lib/rt.jar.bk
	~ jar -uf /Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home/jre/lib/rt.jar java/

		###(check if the permissions are read-write only for root. If so do the next 3 steps, else skip)
	~ ls -lt /Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home/jre/lib/rt.jar
	~ sudo chmod o+r /Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home/jre/lib/rt.jar
	~ sudo chmod g+w /Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home/jre/lib/rt.jar
	~ sudo chmod g+r /Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home/jre/lib/rt.jar

		###(test if this was successful)
	~ javac -bootclasspath .:/usr/lib/jvm/java-6-sun-1.6.0.26/jre/lib/rt.jar -cp . Test.java
	~ java -cp . Test

---------------------------------------------------------------------------------------------------------------------------------------------
README FROM [dileepbapat]: Faketime is to support testing time based features. It allows you to change time to simulate test scenarios.

This is fake time utility, that [Manohar](https://github.com/akula1001) developed. It was using remote python server for time source. 

This fork uses in memory time offset that will be added to real clock. System class is decorated with setTimeOffset and getTimeOffset methods to faketime.

    gcc -shared -I /usr/lib/jvm/java-6-sun-1.6.0.26/include -I /usr/lib/jvm/java-6-sun-1.6.0.26/include/linux/ -m32 jvmfaketime.c  -o libjvmfaketime.so
    cp libjvmfaketime.so /usr/lib/jvm/java-6-sun-1.6.0.26/jre/lib/i386/ 
  [ or whatever your jvm folder is ]

    javac -classpath .:javassist.jar ClassModifier.java 
    java -cp .:javassist.jar  ClassModifier

This will create your own version of java.lang.System class with 4 new native methods to support faketime.

Now update rt.jar so that any java app can use faketime.

    jar -uf /usr/lib/jvm/java-6-sun-1.6.0.26/jre/lib/rt.jar  java/


Compile and run test class to verify the setup.

    javac -bootclasspath .:/usr/lib/jvm/java-6-sun-1.6.0.26/jre/lib/rt.jar -cp . Test.java
    java -cp . Test 