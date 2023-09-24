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
                <div style="background-color: lightblue; height: 50px;" class="flex align-items-center justify-content-center">
                    <span>
                        <Rating v-model="idea.raiting" :cancel="false" disabled />
                    </span>                 
                </div>
                <div style=" height: 30px;"></div>

                <div class="flex flex-column md:flex-row md:justify-content-between row-gap-3">
                    <div>
                        <h2>Сумма</h2>
                        <p>{{ idea.target_sum }}/{{ get_invest_sum(idea.invests) }}</p>
                        <div hidden> {{ idea.percent =  get_procent(idea.target_sum, get_invest_sum(idea.invests))}}</div>
                        <span>
                            <Slider v-model="idea.percent" class="w-14rem" deactivate />
                        </span>

                      
                    </div>
                    <span>
                        <Accordion style="width: 600px;" :activeIndex="0">
                            <AccordionTab header="Инвесторы">
                                <span v-for="invest in idea.invests" :key="invest.id">
                                    <div class="flex flex-column md:flex-row md:justify-content-between row-gap-3">
                                        <span>{{ invest.card.user.username }}</span>
                                        <span>{{ invest.comment }}</span>
                                        <span>{{ invest.sum_of_invest }}</span>

                                    </div>
                                </span>
                            </AccordionTab>
                        </Accordion>
                    </span>
                    <Button type="button" label="Инвестировать" severity="success"></Button>

                </div>
                
                <div>
                    
                </div>

                <div class="flex align-items-center justify-content-center"> 
                    <span v-html="idea.description" ></span>
                </div>
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