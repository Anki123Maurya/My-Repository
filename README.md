# My-Repository
var validFilesTypes = ["doc", "docx", "pdf", "jpg", "jpeg", "png"];
    var extn;
    function CheckExtension(file) {
        var filePath = file.value;
        var ext = filePath.substring(filePath.lastIndexOf('.') + 1).toLowerCase();
        extn = ext
        // alert(ext);
        if (ext != '') {
            var isValidFile = false;
            for (var i = 0; i < validFilesTypes.length; i++) {
                if (ext == validFilesTypes[i]) {
                    isValidFile = true;
                    break;
                }
            }

            if (!isValidFile) {
                file.value = null;
                swal("Only Word, PDF, JPEJ file are allowed.", "", "warning");
            }
        }
        return isValidFile;
    }

    var validFileSize = 4 * 1024 * 1024;

    function CheckFileSize(file) {
        /*global document: false */
        var fileSize = file.files[0].size;
        var isValidFile = false;
        if (fileSize !== 0 && fileSize <= validFileSize) {
            isValidFile = true;
        }
        else {
            file.value = null;
            swal("Size of document should be less than 4 MB.", '', "warning");
        }

        return isValidFile;
    }

    function CheckFile(file) {
        var isValidFile = CheckExtension(file);
        if (isValidFile) {
            isValidFile = CheckFileSize(file);
        }
        return isValidFile;
    }

    //----------------FileUpload for Attachment 1 Start-----------------------

    $("#FileUplod1").change(function () {
        debugger;
        var data = new FormData();
        var files = $("#FileUplod1").get(0).files;
        var validFile = CheckFile(this);
        if (validFile) {
            if (files.length > 0) {
                data.append("MyImages", files[0]);
            }
            $.ajax({
                url: '@Url.Action("UploadFile", "HOME")',
                type: "POST",
                processData: false,
                contentType: false,
                data: data,
                success: function (response) {
                    $('#Attachment1').val('../Content/Attachments/' + response);
                   
                    swal("Uploaded", "Document Uploaded Successfully.", "success");

                },
                error: function (er) {
                    alert(er);
                }

            });
            }
    });
