<template>
    <div>
        
        <Card v-if="idea">
            <template #title> 
                <h1>{{idea.label}} </h1>
                <hr>
                
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

                    <Button label="Инвестировать" icon="pi pi-credit-card" @click="visible = true" severity="success" />

                </div>
                
                <div>
                    
                </div>

                <div class="flex align-items-center justify-content-center"> 
                    <span v-html="idea.description" ></span>
                </div>
            </template>
        </Card>


        <Toast position="to-center" group="bl" />

        <Dialog v-model:visible="visible" modal header="Инвестировать" :style="{ width: '50vw' }">

            <!-- {{ new_invest }} -->
            <div class="card flex flex-column  gap-3">
                <div class="p-inputgroup flex-1">
                   
                    <Dropdown  v-model="new_invest.card" :options="bank_cards" optionLabel="name" placeholder="Выбор карты" class="w-full md:w-14rem">
                        <template #option="slotProps">
                            <div class="flex align-items-center">
                                <div>{{ slotProps.option.name }} - {{ slotProps.option.code }}</div>
                            </div>
                        </template>
                    </Dropdown>
                </div>
                <br>
                <span class="p-float-label">
                    <Textarea v-model="new_invest.comment" rows="5" cols="100"  />
                    <label>Комментарий</label>
                </span>

                <div class="p-inputgroup flex-1">
                    <span class="p-inputgroup-addon">Сумма</span>
                    <InputNumber v-model="new_invest.sum" placeholder="Целевая сумма" />
                    <span class="p-inputgroup-addon">тг</span>
                </div>
                
                <Button label="Отправить" icon="pi pi-check" iconPos="right" @click="on_pop()"  severity="success" />
            </div>
        </Dialog>

    </div>
</template>

<script>
    import axios from 'axios'
    // import { useToast } from "primevue/usetoast";


    export default {
        name: 'app',
        data() {
            return {
                idea_id: this.$route.params.id,
                idea: null,
                visible: false,

                me: null,
                bank_cards: [
                    {   
                        id: 1,
                        name: "Основная карта",
                        code: "4405 6397 8488 5351"
                    },
                    {
                        id: 2,
                        name: "Карта халык",
                        code: "4405 6397 8488 8888"
                    },
                    {
                        id: 3,
                        name: "Корпаративная карта",
                        code: "4405 6397 8488 7777"
                    },
                ],
                
                new_invest: {
                    card: null,
                    comment: null,
                    sum: null,

                },

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
            },

            on_pop(){
                this.visible = false
                this.idea.invests.push(
                {
                    "id": 100,
                    "card": {
                        "id": 1,
                        "user": {
                            "id": 2,
                            "username": "User1",
                            "first_name": "",
                            "last_name": "",
                            "email": ""
                        },
                        "name": "Aisa Rodriguez",
                        "account": "4452678234446898"
                    },
                    "comment": this.new_invest.comment,
                    "create_date": "2023-09-24T13:50:57.901673Z",
                    "sum_of_invest": this.new_invest.sum,
                    "idea": 1
                })
                // toast.add({ severity: 'success', summary: 'Info Message', detail: 'Message Content', group: 'bl', life: 3000 });
            }
        },
        mounted() {
            this.get_idea_by()
            
        }
    }
</script>