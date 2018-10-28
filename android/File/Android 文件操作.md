Android 文件操作
============

### Android 删除文件或文件夹

```java
 
 
	/**
	 * 删除相对路径
	 * */
	public static void deletePath() {
		isHaveSDCard();
		File file;
		if (isHaveSDCard()) {
			file = Environment.getExternalStorageDirectory();
		} else {
			file = Environment.getDataDirectory();
		}
		file = new File(file.getPath() + "/xxx/yyy");
		RecursionDeleteFile(file);
	}
 
	/**
	 * 删除绝对路径
	 * */
	public static void deleteCache() {
		File deleteFile = new File(
				"/sdcard/Android/data/com.xxx/cache");
		RecursionDeleteFile(deleteFile);
		if (!deleteFile.exists()) {
			deleteFile.mkdirs();
		}
	}
 
	/**
	 * 递归删除文件和文件夹
	 * 
	 * @param file
	 *            要删除的根目录
	 */
	public static void RecursionDeleteFile(File file) {
		if (file.isFile()) {
			file.delete();
			return;
		}
		if (file.isDirectory()) {
			File[] childFile = file.listFiles();
			if (childFile == null || childFile.length == 0) {
				file.delete();
				return;
			}
			for (File f : childFile) {
				RecursionDeleteFile(f);
			}
			file.delete();
		}
	}
 
	/** 是否有SD卡 */
	public static boolean isHaveSDCard() {
		String SDState = android.os.Environment.getExternalStorageState();
		if (SDState.equals(android.os.Environment.MEDIA_MOUNTED)) {
			return true;
		}
		return false;
	}

```