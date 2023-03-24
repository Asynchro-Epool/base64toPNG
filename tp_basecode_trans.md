<%*

async function base64Str(base64Str, fileName){
  // 引入 fs 模块
  var fs = require ('fs');
  // 定义 buffer 对象，将 base64 编码转换为二进制数据
  var buffer = Buffer.from (base64Str, 'base64');
  // 使用 fs 模块将 buffer 对象写入到本地文件中
  fs.writeFile (fileName, buffer, function (err) {
    if (err) {
      console.error (err);
    } else {
      console.log ('File created');
    }
  });
}

// 获取当前文件的对象
var currentFile = tp.file;
// 获取当前文件的名称
var fileName = currentFile.title;
// 文件储存的路径
var filePath = "D:/02Documents_database/ObsidianEpool/conf/attachments";
// 将文件内容按换行符分割成数组
var fileLines = currentFile.content.split('\n');
// 获取文件的 tFile
var Tfile = currentFile.find_tfile(fileName)

// 定义一个正则表达式匹配 data:image/png;base64 格式的字符串
var regex = /data:image\/.+?;base64,(.*?)\)/;
// 遍历文件中的每一行，检查是否匹配正则表达式
var nPng = 0
var newFileLines = fileLines.map((line) => {
  if (regex.test(line)) {
    var base64Code = line.match(regex)[1];
    nPng += 1
    var PngfilePath = `${filePath}/${fileName}_${nPng}.png`
    base64Str(base64Code, PngfilePath)
    return `![[${fileName}_${nPng}.png]]`
  }else{
	return line
  }
})

await app.vault.modify(Tfile, newFileLines.join("\n"));
%>