<template>
    <div :id="ed"></div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const emit = defineEmits(["update:content"]);

function generateRandomString(length) {
    const characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";
    let result = "";
    for (let i = 0; i < length; i++) {
        result += characters.charAt(
            Math.floor(Math.random() * characters.length),
        );
    }
    return result;
}

let ed = generateRandomString(10);

function initializeEditor(editorId) {
    CKEditor5.editorClassic.ClassicEditor.create(
        document.querySelector("#" + editorId),
        {
            math: {
                engine: "katex",
                katexRenderOptions: {
                    macros: {
                        "\\test": "\\mathrel{\\char`≠}",
                    },
                },
            },
            plugins: [
                CKEditor5.math.Math,
                CKEditor5.math.AutoformatMath,
                CKEditor5.essentials.Essentials,
                CKEditor5.autoformat.Autoformat,
                CKEditor5.blockQuote.BlockQuote,
                CKEditor5.basicStyles.Bold,
                CKEditor5.heading.Heading,
                CKEditor5.image.Image,
                CKEditor5.image.ImageCaption,
                CKEditor5.image.ImageStyle,
                CKEditor5.image.ImageToolbar,
                CKEditor5.image.ImageUpload,
                CKEditor5.indent.Indent,
                CKEditor5.basicStyles.Italic,
                CKEditor5.link.Link,
                CKEditor5.list.List,
                CKEditor5.mediaEmbed.MediaEmbed,
                CKEditor5.paragraph.Paragraph,
                CKEditor5.table.Table,
                CKEditor5.table.TableToolbar,
                CKEditor5.codeBlock.CodeBlock,
                CKEditor5.basicStyles.Code,
                CKEditor5.upload.Base64UploadAdapter,
            ],
            toolbar: [
                "math",
                "|",
                "heading",
                "|",
                "bold",
                "italic",
                "link",
                "code",
                "bulletedList",
                "numberedList",
                "|",
                "outdent",
                "indent",
                "|",
                "uploadImage",
                "blockQuote",
                "insertTable",
                "mediaEmbed",
                "codeBlock",
                "|",
                "undo",
                "redo",
            ],
            image: {
                toolbar: [
                    "imageStyle:inline",
                    "imageStyle:block",
                    "imageStyle:side",
                    "|",
                    "imageTextAlternative",
                ],
            },
            table: {
                contentToolbar: ["tableColumn", "tableRow", "mergeTableCells"],
            },
        },
    )
        .then((editor) => {
            editor.model.document.on("change:data", () => {
                emit("update:content", editor.getData());
            });
        })
        .catch((error) => {
            console.error(
                "There was a problem initializing the editor.",
                error,
            );
        });
}
onMounted(() => {
    console.log("editor", ed);
    initializeEditor(ed);
});
</script>
