<template>
    <div>
        <Survey
          :srcUrl="srcUrl" :responses="responses[activityIndex]"
          :selected_language="selected_language"
          :progress="progress[activityIndex]"
          :autoAdvance="checkAdvance"
          :actVisibility="visibility"
          :nextActivity="nextActivity"
          v-on:updateProgress="updateProgress"
          v-on:saveResponse="saveResponse"
          v-on:saveScores="saveScores"
          v-on:clearResponses="clearResponses"
        >
            <!--<div>-->
                <!--&lt;!&ndash; everything here is going in the slot &ndash;&gt;-->
                <!--<div v-if="not valid">-->

                    <!--sorry, you dont' qualify-->
                <!--</div>-->
                <!--<div v-else>-->
                    <!--click "next" to continue-->
                <!--</div>-->
            <!--</div>-->
        </Survey>
    </div>
</template>

<script>
import Survey from './components/Survey';


export default {
  name: 'TakeSurvey',
  props: ['srcUrl', 'responses', 'selected_language', 'progress', 'autoAdvance', 'actVisibility', 'nextActivity'],
  // props: remember to pass all of the props from App.vue into our
  // survey component. Also remember to pass on any events that Survey emits
  // to App.vue
  components: {
    Survey,
  },
  data() {

  },
  methods: {
    updateProgress(progress) {
      this.checkProgressDiff(this.progress[this.activityIndex], progress);
      this.$store.dispatch('updateProgress', progress);
      this.$forceUpdate();
    },
    saveResponse(key, value) {
      let needsVizUpdate = false;
      if (this.currentResponse[key] !== value && this.progress[this.activityIndex] === 100) {
        // there has been a change in an already completed activity
        needsVizUpdate = true;
      }
      // const flag = 'response';
      // console.log(150, flag, key, value);
      this.$store.dispatch('saveResponse', { key, value });
      if (needsVizUpdate) {
        this.setVisbility();
      }
      this.isAnswered = true;
    },
    saveScores(key, scoreObj) {
      // console.log(168, key, scoreObj);
      this.$store.dispatch('saveScores', { key, scoreObj });
    },
    clearResponses() {
      this.$store.dispatch('clearResponses', this.activityIndex);
      this.$forceUpdate();
    },
  },
  computed: {
    checkAdvance() {
      if (!_.isEmpty(this.$store.state.schema) && this.$store.state.schema['https://schema.repronim.org/allow']) {
        const allowList = _.map(this.$store.state.schema['https://schema.repronim.org/allow'][0]['@list'],
          u => u['@id']);
        return allowList.includes('https://schema.repronim.org/auto_advance');
      }
      return false;
    },
  },
};
</script>
