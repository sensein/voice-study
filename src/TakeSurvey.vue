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
    components: {
        Survey,
    },
    data() {

    },
    methods: {

    }
}
</script>
