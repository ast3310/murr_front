<template>
  <div class="main-layout-container ">

    <div class="textarea-wrapper mb">
      <resizable-textarea>
        <label>
          <textarea class="murr-header fs" placeholder="Заголовок"></textarea>
        </label>
      </resizable-textarea>
    </div>


    <div class="editorjs-main mb">
      <div id="editorjs">
      </div>
    </div>


    <el-button class="murr-button" @click="save">
      save
    </el-button>

  </div>
</template>

<script>
  import EditorJS from '@editorjs/editorjs';
  import Header from '@editorjs/header';
  import ImageTool from '@editorjs/image';
  import axios from 'axios'
  import {axios_defaults_baseURL} from "../../devAndProdVariables";
  import ResizableTextarea from '../../components/common/ResizableTextarea.js';


  export default {


    mounted() {

      window.editor = new EditorJS({

        autofocus: true,

        tools: {

          header: Header,

          image: {

            class: ImageTool,

            config: {

              additionalRequestHeaders: {
                'Authorization': 'Bearer ' + this.$store.getters.accessToken_getters
              },
              captionPlaceholder: 'Коммент к фото',

              field: 'murr_editor_image',

              endpoints: {
                byFile: axios_defaults_baseURL + '/murr_card/save_editor_image/',
              }
            }
          }
        },
      })
    },
    data: () => ({
      content: {}
    }),

    methods: {

      async save() {

        let x = await window.editor.save();

        const y = {
          data_for_murr: await JSON.stringify(x)
        };

        // eslint-disable-next-line no-console
        console.log(y);


        await axios.post('/murr_card/save_data_for_murr_card/', y);
        // eslint-disable-next-line no-console
        // console.log(x)
        // window.editor.save().then(saved => this.content = saved)
      },
    },

    components: {ResizableTextarea},

    // async beforeCreate() {
    //
    //     const r = await axios.get('murren/tanochka/',
    //         {headers: {'Authorization': 'Bearer ' + this.$store.getters.accessToken_getters}});
    //     this.tanochkaUrl = axios.defaults.baseURL + r.data.img_url;
    //
    // },
    //


  }
</script>

<style scoped>
  .textarea-wrapper {
    width: 100%;

  }

  .murr-header {
    width: 100%;
    max-width: 620px;
    border: 1px solid #AD00FF;
    background-color: #004dda;
    border-radius: 5px;
    text-align: center;
    height: 58px;
    resize: none;
    padding: 5px;
    overflow: hidden;
    font-family: JetBrainsMono-Italic, sans-serif;

  }

  .editorjs-main {
    width: 100%;
    max-width: 620px;
    border: 1px solid #AD00FF;
    background-color: #004dda;
    border-radius: 5px;
    text-align: left;
    padding: 10px;
  }


  @media only screen and (min-width: 650px) {
    .editorjs-main {
      padding-left: 60px;
    }
  }

</style>
