<template>
    <div class='sc-message--file'  :style="messageColors">
        <div v-for="(img ,index) in data.file" :key="index">
            <div       v-if="img.indexOf('.gif') > 0|| img.indexOf('png') > 0 ||img.indexOf('.jpg') > 0|| img.indexOf('.jpeg') > 0"     class='sc-message--file-icon'>
                <img :src="img" class="sc-image">
            </div>


            <div class="file"
                 v-if="img.indexOf('.xlsx') > 0|| img.indexOf('xls') > 0 || img.indexOf('.docx') > 0|| img.indexOf('.doc') > 0 || img.indexOf('.pdf') > 0">
                <img src="../assets/Download.svg" alt="">
                <div>
                    <p>{{img.slice(-4)}}</p>
                    <a :href="img">Скачать </a>
                </div>
            </div>
        </div>



<!--        <div class='sc-message&#45;&#45;file-name' :style="messageColors">-->
<!--            <a :href="data.file ? data.file : '#'" target='_blank'>Скачать Фаил</a>-->
<!--        </div>-->
        <div class="sc-message--file-text" :style="messageColors">{{data.text}}<p v-if="data.meta"
                                                                                  class='sc-message--meta'
                                                                                  :style="messageColors">
            {{data.meta}}</p></div>
    </div>
</template>

<script>
    export default {
        props: {
            data: {
                type: Object,
                required: true
            },
            messageColors: {
                type: Object,
                required: true
            }
        },
        mounted() {
            {
                {
                    data.file
                }
            }
        }
    }
</script>

<style scoped>
    .sc-message--file {
        border-radius: 6px;
        font-weight: 300;
        font-size: 14px;
        line-height: 1.4;
        /* white-space: pre-wrap; */
        -webkit-font-smoothing: subpixel-antialiased;
        align-items: center;
    }

    .sc-message--content.sent .sc-message--file {
        word-wrap: break-word;
    }

    .sc-message--file-icon {
        text-align: center;
        margin-left: auto;
        margin-right: auto;
        margin-top: 15px;
        margin-bottom: 0px;
    }

    .sc-image {
        max-width: 100%;
        min-width: 100%;
    }

    .sc-message--file-text {
        /*padding: 17px 20px;*/
        border-radius: 6px;
        font-weight: 300;
        font-size: 14px;
        line-height: 1.4;
        white-space: pre-wrap;
        -webkit-font-smoothing: subpixel-antialiased
    }

    .sc-message--file-name {
        color: white;
        padding-left: 15px;
        padding-right: 15px;
        padding-top: 0;
        font-size: x-small;
        text-align: center;
    }

    .sc-message--file-name a {
        text-decoration: none;
        font-size: 15px;
        color: #4e4949;
    }

    .sc-message--file-name a:hover {
        color: #454545;
    }

    .sc-message--content.sent .sc-message--file-text {
        color: white;
        background-color: #4e8cff;
        word-wrap: break-word;
    }

    .sc-message--content.received .sc-message--file {
        color: #263238;
        background-color: #f4f7f9;
        margin-right: 40px;
    }

    .sc-message--content.received .sc-message--file-name {
        color: #000;
    }

    .sc-message--content.received .sc-message--file a {
        color: rgba(43, 40, 40, 0.7);
    }

    .sc-message--content.received .sc-message--file a:hover {
        color: #0c0c0c;
    }

    .file {
        margin: 5px;
        display: flex;
        align-items: center;
    }

    .file a {

        text-decoration: none;
        color: #A6BCD6;
        font-size: 12px;
        font-family: Roboto;
        cursor: pointer;
    }

    .file div {
        display: flex;
        flex-direction: column;
        margin-left: 20px;
        margin-right: 20px;
    }

    .file div p {
        margin-bottom: 5px
    }


    .file img {
        height: 32px;
        width: 32px;

    }

</style>
