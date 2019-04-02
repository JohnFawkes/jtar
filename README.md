# Overview
JTar is a simple Java Tar library, that provides an easy way to create and read tar files using IO streams. The API is very simple to use and similar to the *java.util.zip* package. This repo is forked from [kamranzafar/jtar](https://github.com/kamranzafar/jtar) with modifications to build on Android and switch to use **GNU USTAR tar header**.

[![](https://jitpack.io/v/topjohnwu/jtar.svg)](https://jitpack.io/#topjohnwu/jtar)

## Usage
JTar is available on Jitpack and can be added as a dependency.
```
repositories {
    ...
    maven { url 'https://jitpack.io' }
}
dependencies {
    implementation 'com.github.topjohnwu:jtar:1.0.0'
}
```

Below are some examples of using jtar in applications

### Tar example - using TarOutputStream
<pre><code>  // Output file stream
  FileOutputStream dest = new FileOutputStream( "c:/test/test.tar" );
  
  // Create a TarOutputStream
  TarOutputStream out = new TarOutputStream( new BufferedOutputStream( dest ) );
  
  // Files to tar
  File[] filesToTar=new File[2];
  filesToTar[0]=new File("c:/test/myfile1.txt");
  filesToTar[1]=new File("c:/test/myfile2.txt");
  
  for(File f:filesToTar){
     out.putNextEntry(new TarEntry(f, f.getName()));
     BufferedInputStream origin = new BufferedInputStream(new FileInputStream( f ));
     int count;
     byte data[] = new byte[2048];
  
     while((count = origin.read(data)) != -1) {
        out.write(data, 0, count);
     }
  
     out.flush();
     origin.close();
  }
  
  out.close();
</code></pre>

### Untar example - using TarInputStream
<pre><code>  String tarFile = "c:/test/test.tar";
  String destFolder = "c:/test/myfiles";
  
  // Create a TarInputStream
  TarInputStream tis = new TarInputStream(new BufferedInputStream(new FileInputStream(tarFile)));
  TarEntry entry;
  
  while((entry = tis.getNextEntry()) != null) {
     int count;
     byte data[] = new byte[2048];
     FileOutputStream fos = new FileOutputStream(destFolder + "/" + entry.getName());
     BufferedOutputStream dest = new BufferedOutputStream(fos);
  
     while((count = tis.read(data)) != -1) {
        dest.write(data, 0, count);
     }
  
     dest.flush();
     dest.close();
  }
  
  tis.close();
</code></pre>

___Tip: Always use buffered streams with jtar to speed up IO.___
