<template>
    <div>
       
        
        <Card v-if="idea">
            <template #title> 
                <h1>{{idea.label}} </h1>
                
                <small>
                    {{ idea.short_description }}
                </small>
            </template>
            <template #content>
                <div style="background-color: lightblue;">
                    sfaskdjfkljlksdf
                </div>
                
                <span v-html="idea.description" >
                </span>
            </template>
        </Card>

    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: 'app',
        data() {
            return {
                idea_id: this.$route.params.id,
                idea: null,

                me: null,
                bank_cards: []
            };
        },
        methods:{
            async get_idea_by(){
                try {
                    const info = await axios.get(`http://127.0.0.1:8000/blog/idea/${this.idea_id}`, {Authorization: 'Basic YWRtaW46YWRtaW4='});
                    this.idea = info.data
                    console.log(">>> start idea")
                    console.log(this.idea);
                } catch (error) {
                    console.error(error);
                }
            },  
            
            get_invest_sum(invest_list){
                let sum = 0
                invest_list.forEach(invest => {
                    sum += invest.sum_of_invest
                })
                return sum
            },

            get_procent(target_sum, invested){
                return 100* (invested/ target_sum)
            },

            formate_date(date){
                console.log( date);
                return date.substring(0, 10);
            }
        },
        mounted() {
            this.get_idea_by()
        }
    }
</script>