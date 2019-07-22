<template>
  <div class="textInput">
    <!--<b-button v-if="!isRecording && !hasRecording" @click="record" variant="danger">-->
      <!--record-->
    <!--</b-button>-->
    <!--<div v-if="isRecording">-->
      <!--<small>{{timeRemaining}} seconds left</small>-->
    <!--</div>-->
    <b-button class="align-middle" @click="uploadZipData"
              >Upload</b-button>
    <!--<b-form @submit="onSubmit">-->
      <!--<b-btn type="submit">Upload</b-btn>-->
    <!--</b-form>-->
  </div>
</template>

<style>
</style>

<script>
import _ from 'lodash';
import JSZip from 'jszip';
import axios from 'axios';

export default {
  name: 'SaveData',
  props: ['constraints', 'init', 'selected_language'],
  data() {
    return {
      input: '',
    };
  },
  computed: {
    showResult() {
      if (this.result) {
        this.$emit('valueChanged', this.result);
        return 'Success';
      } return 'Fail';
    },
  },
  methods: {
    uploadZipData() {
      console.log('26, responses', this.$store.state.responses);
      const Response = this.$store.state.responses;
      const totalScores = this.$store.state.scores;
      const totalResponse = { response: Response, scores: totalScores };
      this.formatData(totalResponse);
    },
    formatData(data) {
      const jszip = new JSZip();
      const fileUploadData = {};
      const JSONdata = {};
      const JSONscores = {};
      // sort out blobs from JSONdata
      _.map(data.response, (val, key) => {
        if (val instanceof Blob) {
          fileUploadData[key] = val;
        } else if (_.isObject(val)) {
          // make sure there aren't any Blobs here.
          // if there are, add them to fileUploadData
          _.map(val, (val2, key2) => {
            if (val2 instanceof Blob) {
              fileUploadData[`${key2}`] = val2;
            } else {
              // refill the object.
              if (!JSONdata[key]) {
                JSONdata[key] = {};
              }
              JSONdata[key][key2] = val2;
            }
          });
        } else {
          JSONdata[key] = val;
        }
      });
      _.map(data.scores, (val, key) => { // todo: check if score object not null?
        if (!_.isEmpty(val)) {
          JSONscores[key] = val;
        }
      });
      _.map(JSONdata, (val, key) => {
        jszip.folder('data/responses').file(`activity_${key}.json`, JSON.stringify(val, null, 4));
      });
      _.map(JSONscores, (val, key) => {
        jszip.folder('data/scores').file(`activity_${key}_score.json`, JSON.stringify(val, null, 4));
      });
      let f = 0;
      _.map(fileUploadData, (val) => {
        jszip.folder('data').file(`voice_item_${f + 1}.wav`, val);
        f += 1;
      });
      jszip.generateAsync({ type: 'blob' })
        .then((myzipfile) => {
          // saveAs(myzipfile, 'study-data.zip');
          // axios.post('http://localhost:8000/check', JSONscores[0], {
          axios.post('https://sig.mit.edu/vb/check', JSONscores[0], {
            ContentType: 'application/json',
          })
            .then((response) => {
              const formData = new FormData();
              formData.append('file', myzipfile);
              formData.append('token', response.data.token);
              // axios.post('http://localhost:8000/submit', formData, {
              axios.post('https://sig.mit.edu/vb/submit', formData, {
                'Content-Type': 'multipart/form-data',
              }).then((res) => {
                console.log('SUCCESS!!', res);
              })
                .catch(() => {
                  console.log('FAILURE!!');
                });
            })
            .catch((error) => {
              console.log(error);
            });
        });

      // .then((content) => {
      //   saveAs(content, 'study-data.zip');
      // });
    },
  },
};
</script>
