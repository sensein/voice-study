<template>
    <div>
        <Survey ...props>
            <div>
                <!-- everything here is going in the slot -->
                <div v-if="not valid">

                    sorry, you dont' qualify
                </div>
                <div v-else>
                    click "next" to continue
                </div>
            </div>
        </Survey>
    </div>
</template>

<script>
import Survey from './components/Survey';


export default {
    name: 'TakeSurvey',
    //props: remember to pass all of the props from App.vue into our
    // survey component. Also remember to pass on any events that Survey emits
    // to App.vue
    components: {
        Survey,
    },
    data() {

    },
    methods: {
        updateProgress(args_from_survey) {
            this.$emit('updateProgress', args_from_survey);
        }
    }
}
</script>
