# hta-js-excel-open-filename
Excel.Application の GetOpenFilename でファイルのパスを取得します
## add settings.json ( Code Runner )
```
    "code-runner.showRunIconInEditorTitleMenu": false,
    "code-runner.executorMapByFileExtension": {
        ".hta": "C:\\Windows\\SysWOW64\\mshta.exe",
        ".htm": "C:\\Windows\\SysWOW64\\mshta.exe"
    }
```
```javascript
var excel = null;

$(function(){

    // Excel でファイルを開くダイアログ	
    $("#open_file_dialog").on("click", function(){
    
        // Excel をロード
        excel = new ActiveXObject("Excel.Application");

        // 表示
        excel.Visible = true;

        // 最小化して元のサイズと位置 : GetSaveAsFilename を前面に出す為
        // WScript.Shell の Run と同じ 2 と 1 が使える
        excel.WindowState = 2	// 最小化
        excel.WindowState = 1	// 元のサイズと位置

        excel.DisplayAlerts = false;

        // 一つのファイルを開く
        // https://docs.microsoft.com/ja-jp/office/vba/api/excel.application.getopenfilename
        var filePath = excel.GetOpenFilename("全て,*.*,CSV,*.csv", 1,"ファイルの選択",null, false );
        // 非表示
        excel.Visible = false;

        // 未選択の場合
        if( filePath === false ) {
            alert("ファイルの参照選択がキャンセルされました")
        }
        // 選択の場合
        else {
            alert(filePath + " を選択しました");
        }

        // Excel を終了
        excel.Quit();
        excel = null;
        // Excel 解放
        var idTmr = window.setTimeout("Cleanup();",1);

    });

});
// ******************************
// Excel 解放
// ******************************
function Cleanup() {
    CollectGarbage();
}
```
