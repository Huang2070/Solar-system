MultipartFile是Spring提供的一个接口，用来接收multipart／form-data类型 请求方式中即将上传的文件，为处理或保存文件，MultipartFile和File需要经常进行转换。

1、MultipartFile文件转为File文件（此处未写文件为空等判断）：首先新建一个File类型的文件，然后获取到上传的文件流，以流的方式将上传的文件输出到File文件即可。

```Java
private File transFile(MultipartFile multipartFile) {
        //文件上传前的名称
        String fileName = multipartFile.getOriginalFilename();
        File file = new File(fileName);
        OutputStream out = null;
        try{
            out = new FileOutputStream(file);
            byte[] ss = multipartFile.getBytes();
            for(int i = 0; i < ss.length; i++){
                out.write(ss[i]);
            }
        }catch(IOException e){
            e.printStackTrace();
        }finally {
            if (out != null){
                try {
                    out.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return file;
    }
```


注意：这时候，系统会在根目录下创建一个临时文件，这个临时文件并不是我们需要的，所以文件处理完成之后，需要将其删除。

```Java
// 操作完上的文件 需要删除在根目录下生成的文件
File f = new File(file.toURI());
if (f.delete()) {
    System.out.println("删除成功");
}else {
    System.out.println("删除失败");
}
```


2、File文件转为MultipartFile文件(注意：未判空，请自己添加)
```Java
private MultipartFile getMultipartFile(File file) {
    FileInputStream fileInputStream = null;
    MultipartFile multipartFile = null;
    try {
        fileInputStream = new FileInputStream(file);
        multipartFile = new MockMultipartFile(file.getName(),file.getName(),
                            ContentType.APPLICATION_OCTET_STREAM.toString(),fileInputStream);
    } catch (Exception e) {
        e.printStackTrace();
    }
    return multipartFile;
}
```
