# GetBlock.md
First Big Data
import java.io.FileNotFoundException;
import java.io.IOException;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.BlockLocation;
import org.apache.hadoop.fs.FSDataInputStream;
import org.apache.hadoop.fs.FileStatus;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IOUtils;


public class GetBlock{

	/**
	 * @param args
	 */
	public static FileSystem fs;
	public static FileSystem hdfs;
	public static void init() throws IOException{
		Configuration conf=new Configuration();
		hdfs=FileSystem.get(conf);
		fs=FileSystem.getLocal(conf);
	}
	public static void get() throws FileNotFoundException, IllegalArgumentException, IOException{
		FSDataInputStream ifs=hdfs.open(new Path("hdfs:///user/hadoop-2.6.1.tar.gz"));
//		FileStatus[] ft=hdfs.listFiles((new Path("hdfs:///user/hadoop-2.6.1.tar.gz"),true);
//		hdfs.listFiles(new Path("hdfs:///user/hadoop-2.6.1.tar.gz"),true);
		FileStatus[] ft=hdfs.listStatus(new Path("hdfs:///user/hadoop-2.6.1.tar.gz"));
//		ft[0].getLen();
		BlockLocation[] gg=hdfs.getFileBlockLocations(new Path("hdfs:///user/hadoop-2.6.1.tar.gz"), 0,ft[0].getLen());
	    long g=gg[1].getOffset();
	    long zz=gg[1].getLength();
	    //System.out.println();
	    System.out.println(ft.length);
	    System.out.println(gg.length);
	    System.out.println(gg[0].getOffset());	
		System.out.println(gg[0].getLength());	
		System.out.println(gg[1].getOffset());	
		System.out.println(gg[1].getLength());	
		ifs.seek(g);
		IOUtils.copyBytes(ifs,fs.create(new Path("file:///usr/3.tar.gz")),zz,true);
		ifs.close();
		fs.close();
		hdfs.close();
	}
	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
   init();
   get();
	}

}

