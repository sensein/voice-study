<template>
  <div class="docked-layout">
    <section>
      <div class="container">
        <div class="screen" v-show="step === 1">
          <div class="panel">
            <BridgeImage src="/static/images/about%20the%20study.svg"/>

            <h3>About the study</h3>
            <p>Researchers at MIT and the University of Washington are currently seeking volunteers
              to participate in a study investigating how voice changes are related to your mental
              health state. Specifically, we want to study how your voice changes in response to
              your mental health state and need to collect voice recordings of many people across
              varying levels of depression, so that we can understand this relationship better.
              We need your help in order to collect this data. This is a fully remote online study.
            </p>
          </div>
        </div>
        <div class="screen" v-show="step === 2">
          <div class="panel">
            <BridgeImage src="/static/images/procedures%20activities.svg"/>
            <h3>How does the study work?</h3>
            <p>
              1. Take the screening test to determine if you are eligible for the study <br>
              2. Complete the informed consent process<br>
              3. You will be asked to provide audio recordings of multiple voice tasks
              and will be required to have a working microphone for these tasks. <br>
              4. Complete a demographics questionnaire and surveys related to your mood and health.
              <br>
            </p>
          </div>
        </div>
        <div class="screen" v-show="step === 3">
          <div class="panel">
            <BridgeImage src="/static/images/how%20long%20does%20it%20last.svg"/>
            <h3>How long does it last?</h3>
            <p>The study is expected to take no more than 15 minutes to complete. You may decline to
              answer any question and may withdraw from the study at any time with no penalty.</p>
          </div>
        </div>
        <div class="screen" v-show="step === 4">
          <div class="panel">
            <BridgeImage src="/static/images/benefits%20and%20risks.svg"/>
            <h3>What are the benefits and risks?</h3>
            <p>You may not directly benefit from participating in the study but your data may be
              interesting to you.</p>

            <p>You will also be helping researchers advance the current understanding of depression
              treatment.</p>
          </div>
        </div>
        <div class="screen" v-show="step === 5">
          <div class="panel">
            <BridgeImage src="/static/images/mit_voice_pilot_applet_image.svg"/>
            <h3>Let’s get started!</h3>
            <p>Now that you’ve learned the basics of the study, we will provide you with a bit more
              detail so you can decide if it is right for you.</p>

            <p>First, let’s see if you’re eligible for the voice study!</p>
          </div>
        </div>
      </div>
    </section>
    <div class="buttons">
      <button @click="doBack" :disabled="this.step === 1">Back</button>
      <button @click="doNext">{{nextName}}</button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'StudyIntroduction',
  data() {
    return {
      step: 1,
      totalSteps: 5,
    };
  },
  computed: {
    nextName() {
      return (this.step === this.totalSteps) ? 'Start' : 'Next';
    },
    appletURL() {
      return 'https://raw.githubusercontent.com/ReproNim/reproschema/master/activity-sets/VoicePilot/VoicePilot_schema';
    },
    redirect() {
      return { name: 'Applet', params: { appletId: this.appletURL }, query: { ...this.query, consent: true } };
    },
  },
  methods: {
    doBack() {
      if (this.step > 1) {
        this.step -= 1;
      }
    },
    doNext() {
      if (this.step < this.totalSteps) {
        this.step += 1;
      } else if (this.step === this.totalSteps) {
        this.$router.push('/activities/0');
      }
    },
    learnMore() {
      this.$refs.consentViewer.toggleMax();
    },
  },
};
</script>

<style scoped>
  section {
    padding-top: 0;
  }
  .container {
    max-width: 30rem;
    margin: 0 auto;
    padding: 0 1.5rem;
  }
  .screen {
    margin: 0 auto;
    text-align: center;
  }
  .panel {
    overflow-y: auto;
  }
  .consent-viewer {
    width: 0px;
    height: 0px!important;
  }
  .root {
    max-width: 40rem;
    margin: 0 auto;
  }
  h3 {
    font-size: 3.2vh;
    font-weight: bold;
  }
  p, a {
    font-size: calc(2vh*1.4);
  }
  h3 {
    margin: 0 0 .5rem 0;
  }

  p {
    text-align: center;
  }
  img {
    height: 36vh;
    display: block;
    margin: 1rem auto;
  }
  .buttons {
    margin: 1rem auto 5vh auto;
    text-align: center;
  }
  .buttons button {
    font-size: 1rem;
    background-color: white;
    padding: .75rem 2rem;
    border-radius: 100px;
    cursor: pointer;
    font-weight: bold;
  }
  .buttons button + button {
    margin-left: 1rem;
  }
  button[disabled] {
    opacity: .5;
  }
  @media screen and (max-width: 50em) {
    h3, p {
      line-height: 1.1;
    }
  }
</style>
