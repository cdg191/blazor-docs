---
layout: post
title: Load office files in Blazor PDF Viewer | Syncfusion
description: Learn here all about how to load the microsoft office files like powerpoint in the Syncfusion Blazor PDF Viewer component and more.
platform: Blazor
control: PDF Viewer
documentation: ug
---

# Load Microsoft Office files in Blazor PDF Viewer Component

The PDF Viewer library allows you to load ms office files such as powerpoint, excel, word and image using the `GetImageStream()` method of controller.

The following steps are used to load the office files in the PDF Viewer.

```cshtml

@page "/"
@using Syncfusion.Blazor.PdfViewerServer
@using Syncfusion.Blazor.PdfViewer
@using Syncfusion.Pdf;
@using Syncfusion.Pdf.Graphics;
@using Syncfusion.Pdf.Interactive;
@using Syncfusion.Pdf.Parsing;
@using Syncfusion.Pdf.Security;
@using Syncfusion.Blazor.Inputs;
@using Syncfusion.DocIO;
@using WFormatType = Syncfusion.DocIO.FormatType;
@using Syncfusion.Presentation;
@using Syncfusion.PresentationRenderer;
@using Syncfusion.XlsIO;
@using Syncfusion.XlsIORenderer
@using Syncfusion.DocIORenderer;
@using System.IO;

<SfUploader AllowedExtensions=".doc, .docx, .rtf, .docm, .dotm, .dotx, .dot, .xls, .xlsx, .pptx, .pptm, .potx, .potm .jpeg, .png, .bmp">
    <UploaderEvents OnUploadStart="OnSuccess"></UploaderEvents>
    <UploaderAsyncSettings SaveUrl="https://aspnetmvc.syncfusion.com/services/api/uploadbox/Save"
                           RemoveUrl="https://aspnetmvc.syncfusion.com/services/api/uploadbox/Remove"></UploaderAsyncSettings>
</SfUploader>
<SfPdfViewerServer @ref="viewerInstance" Height="500px" Width="1060px" ToolbarSettings="@ToolbarSettings"></SfPdfViewerServer>

@code {
    SfPdfViewerServer viewerInstance;

    public PdfViewerToolbarSettings ToolbarSettings = new PdfViewerToolbarSettings()
        {
            ToolbarItems = new List<ToolbarItem>()
            {
                ToolbarItem.PageNavigationTool,
                ToolbarItem.MagnificationTool,
                ToolbarItem.CommentTool,
                ToolbarItem.SelectionTool,
                ToolbarItem.PanTool,
                ToolbarItem.UndoRedoTool,
                ToolbarItem.CommentTool,
                ToolbarItem.AnnotationEditTool,
                ToolbarItem.SearchOption,
                ToolbarItem.PrintOption,
                ToolbarItem.DownloadOption,
                ToolbarItem.SubmitForm,
            }
        };

    private void OnSuccess(UploadingEventArgs action)
    {
        string base64 = action.FileData.RawFile.ToString();
        //string fileName = args.FileData[0].Name;
        string type = action.FileData.Type.ToLower();
        string data = base64.Split(',')[1];
        byte[] bytes = Convert.FromBase64String(data);
        var outputStream = new MemoryStream();
        Syncfusion.Pdf.PdfDocument pdfDocument = new Syncfusion.Pdf.PdfDocument();
        using (Stream stream = new MemoryStream(bytes))
        {
            switch (type)
            {
                case "docx":
                case "dot":
                case "doc":
                case "dotx":
                case "docm":
                case "dotm":
                case "rtf":
                    Syncfusion.DocIO.DLS.WordDocument doc = new Syncfusion.DocIO.DLS.WordDocument(stream, GetWFormatType(type));
                    //Instantiation of DocIORenderer for Word to PDF conversion
                    DocIORenderer render = new DocIORenderer();
                    //Converts Word document into PDF document
                    pdfDocument = render.ConvertToPDF(doc);
                    doc.Close();
                    break;
                case "pptx":
                case "pptm":
                case "potx":
                case "potm":
                    //Loads or open an PowerPoint Presentation
                    IPresentation pptxDoc = Presentation.Open(stream);
                    pdfDocument = PresentationToPdfConverter.Convert(pptxDoc);
                    pptxDoc.Close();
                    break;
                case "xlsx":
                case "xls":
                    ExcelEngine excelEngine = new ExcelEngine();
                    //Loads or open an existing workbook through Open method of IWorkbooks
                    IWorkbook workbook = excelEngine.Excel.Workbooks.Open(stream);
                    //Initialize XlsIO renderer.
                    XlsIORenderer renderer = new XlsIORenderer();
                    //Convert Excel document into PDF document
                    pdfDocument = renderer.ConvertToPDF(workbook);
                    workbook.Close();
                    break;
                case "jpeg":
                case "jpg":
                case "png":
                case "bmp":
                    //Add a page to the document
                    PdfPage page = pdfDocument.Pages.Add();
                    //Create PDF graphics for the page
                    PdfGraphics graphics = page.Graphics;
                    PdfBitmap image = new PdfBitmap(stream);
                    //Draw the image
                    graphics.DrawImage(image, 0, 0);
                    break;
            }
            pdfDocument.Save(outputStream);
            outputStream.Position = 0;
            loadPDFdocument(outputStream.ToArray());
            pdfDocument.Close();
            outputStream.Close();
        }
    }

    public void loadPDFdocument(byte[] bytes)
    {
        string base64String = Convert.ToBase64String(bytes);
        viewerInstance.LoadAsync("data:application/pdf;base64," + base64String, null);
    }

    public static WFormatType GetWFormatType(string format)
    {
        if (string.IsNullOrEmpty(format))
            throw new NotSupportedException("EJ2 DocumentEditor does not support this file format.");
        switch (format.ToLower())
        {
            case "dotx":
                return WFormatType.Dotx;
            case "docx":
                return WFormatType.Docx;
            case "docm":
                return WFormatType.Docm;
            case "dotm":
                return WFormatType.Dotm;
            case "dot":
                return WFormatType.Dot;
            case "doc":
                return WFormatType.Doc;
            case "rtf":
                return WFormatType.Rtf;
            default:
                throw new NotSupportedException("EJ2 DocumentEditor does not support this file format.");
        }
    }

}
```

Add the following code in the controller.cs file in the web service project to load the ms office files.

```c#
//Post action for loading the Office products
public IActionResult GetImageStream([FromBody] Dictionary<string, string> jsonObject)
{
    if (jsonObject.ContainsKey("data"))
    {
        string base64 = jsonObject["data"];
        //string fileName = args.FileData[0].Name;
        string type = jsonObject["type"];
        string data = base64.Split(',')[1];
        byte[] bytes = Convert.FromBase64String(data);
        var outputStream = new MemoryStream();
        Syncfusion.Pdf.PdfDocument pdfDocument = new Syncfusion.Pdf.PdfDocument();
        using (Stream stream = new MemoryStream(bytes))
        {
            switch (type)
            {
                case "docx":
                case "dot":
                case "doc":
                case "dotx":
                case "docm":
                case "dotm":
                case "rtf":
                    Syncfusion.DocIO.DLS.WordDocument doc = new Syncfusion.DocIO.DLS.WordDocument(stream, GetWFormatType(type));
                    //Initialization of DocIORenderer for Word to PDF conversion
                    DocIORenderer render = new DocIORenderer();
                    //Converts Word document into PDF document
                    pdfDocument = render.ConvertToPDF(doc);
                    doc.Close();
                    break;
                case "pptx":
                case "pptm":
                case "potx":
                case "potm":
                    //Loads or open an PowerPoint Presentation
                    IPresentation pptxDoc = Presentation.Open(stream);
                    pdfDocument = PresentationToPdfConverter.Convert(pptxDoc);
                    pptxDoc.Close();
                    break;
                case "xlsx":
                case "xls":
                    ExcelEngine excelEngine = new ExcelEngine();
                    //Loads or open an existing workbook through Open method of IWorkbooks
                    IWorkbook workbook = excelEngine.Excel.Workbooks.Open(stream);
                    //Initialize XlsIO renderer.
                    XlsIORenderer renderer = new XlsIORenderer();
                    //Convert Excel document into PDF document
                    pdfDocument = renderer.ConvertToPDF(workbook);
                    workbook.Close();
                    break;
                case "jpeg":
                case "jpg":
                case "png":
                case "bmp":
                    //Add a page to the document
                    PdfPage page = pdfDocument.Pages.Add();
                    //Create PDF graphics for the page
                    PdfGraphics graphics = page.Graphics;
                    PdfBitmap image = new PdfBitmap(stream);
                    //Draw the image
                    graphics.DrawImage(image, 0, 0);
                    break;
            }

        }
        pdfDocument.Save(outputStream); 
        outputStream.Position = 0;
        byte[] byteArray= outputStream.ToArray();
        pdfDocument.Close();
        outputStream.Close();

        string base64String = Convert.ToBase64String(byteArray); 
        return Content("data:application/pdf;base64," + base64String);     
    }
    return Content("data:application/pdf;base64," + "");
}
        
```

[View sample in GitHub](https://github.com/SyncfusionExamples/blazor-pdf-viewer-examples/tree/master/Common/Load%20PDF%2C%20Excel%2C%20PPT%20file%20types).