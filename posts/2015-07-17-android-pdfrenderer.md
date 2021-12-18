layout: post
title: "Render PDF on Android"
date: 2015-07-17 09:39:57 +0800
comments: true
catetory: Android
Tags: Pdf
Has_Math: True


因為有顯示 PDF 文件的需求, 之前也沒有相關經驗, 上網爬了一些資料, 才發現許多的方案, 因為 Google 到 Android 5.0 以前, 都沒有支援這部分, 所以出現許多第三方的 libs, 比較知名的有：

* iText: [http://itextpdf.com](http://itextpdf.com)
* MuPDF: [http://www.mupdf.com/](http://http://www.mupdf.com/)
* Android-Pdf-Viewer [https://github.com/jblough/Android-Pdf-Viewer-Library](https://github.com/jblough/Android-Pdf-Viewer-Library)

從 Android 5.0 起, SDK 有內建的 PdfRenderer class, 使用範例在 [Github PdfRenderBasic](https://github.com/googlesamples/android-PdfRendererBasic).

<!--More-->

先試用一下 Android-Pdf-Viewer, 我的開發環境是 Android Studio:

* 首先從 前面提到的 GitHub 下載 project.
* `File`|`New`|`New Module` 後出現這個 Dialog:

![](/images/Import Jar Dialog.png)

* 選擇 Import .JAR or .AAR Package, 然後從 Android-Pdf-Viewer 裡面找到 jar

![](/images/select imported jar.png)

* 稍微等待 Gradle 重建關係, 可以看到 project manager 新增了 jar module:

![](/images/new jar added as a module.png)

* 接下來的步驟, 依照該專案的 [Readme](https://github.com/googlesamples/android-PdfRendererBasic) 提示, 依次加上, 這裡就不重複過程了.

使用結果：

__失敗,開檔案時停留在尋找檔案的畫面__

StackOverflow [有人也有相同的現象：](http://stackoverflow.com/questions/19478168/android-pdf-viewer-library-or-mupdf-library-tutorials).

所以, 轉而使用內建的 PdfRenderer:

這個 Example 雖然可以顯示 PDF, 但是 porting 之後才發現他有這個問題：

		apk 內放置 pdf 受到其他檔案的編排位置不同, 發生讀檔時的 exception: not create document. Error

這裏是討論到這個問題的 Bugzilla : [https://bugzilla.xamarin.com/show_bug.cgi?id=25397](https://bugzilla.xamarin.com/show_bug.cgi?id=25397)

原因出在 PdfRenderer 身上, 雖然我們在 build.gradle 裡面, 也依照 sample project 的方式, 註明不要壓縮 pdf:

		    aaptOptions {
        noCompress "pdf"
    }

但是沒有幫助, work around 就是把 .pdf copy 一遍, 然後讀取 copy 後的 PDF.
很爛的解決方式, 等待 Google 修復之前只好這樣了, 把原先的 openRenderer() 改一下:


修改前：

		:::Objective-C

		private void openRenderer(Context context) throws IOException {

        // In this sample, we read a PDF from the assets directory.
        mFileDescriptor = context.getAssets().openFd("sample.pdf").getParcelFileDescriptor();

        // This is the PdfRenderer we use to render the PDF.
        mPdfRenderer = new PdfRenderer(mFileDescriptor);
    }



修改後：

		:::Objective-C

		private void openRenderer(Context context) throws IOException {

        // In this sample, we read a PDF from the assets directory.
        InputStream ins = context.getAssets().open("sample.pdf");
        OutputStream ous = context.openFileOutput("_sample.pdf", Context.MODE_PRIVATE);
        copyFileUsingFileStreams(ins, ous);

        mFileDescriptor = ParcelFileDescriptor.dup(context.openFileInput("_sample.pdf").getFD());

        // This is the PdfRenderer we use to render the PDF.
        mPdfRenderer = new PdfRenderer(mFileDescriptor);
    }


        private static void copyFileUsingFileStreams(InputStream fin, OutputStream fout)
            throws IOException {
        try {
            byte[] buf = new byte[1024];
            int bytesRead;
            while ((bytesRead = fin.read(buf)) > 0) {
                fout.write(buf, 0, bytesRead);
            }
        } finally {
            fin.close();
            fout.close();
        }
    }


 這樣就可以用了！


