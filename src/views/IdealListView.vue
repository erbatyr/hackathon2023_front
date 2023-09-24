<template>
    <div>

        <Dropdown v-model="category_select" 
        :options="category_list" 
        @change="d"
        optionLabel="name" placeholder="Категория" class="w-full md:w-14rem" />
        


        <DataTable :value="idea_list" tableStyle="min-width: 50rem">
            <template #header>
                <div class="flex flex-wrap align-items-center justify-content-between gap-2">
                    <span class="text-xl text-900 font-bold">Список идей</span>
                    <!-- <Button icon="pi pi-refresh" rounded raised /> -->
                </div>
            </template>
            <Column field="label" sortable header="Название">
                <template #body="slotProps">

                    <h3>
                        {{ slotProps.data.id }}
                        <router-link :to="{ name: 'idea_page', params: { id: slotProps.data.id }}">{{ slotProps.data.label }}</router-link>
                    </h3>
                    <p>{{ slotProps.data.short_description }} </p>
                    <small>Категория: {{ slotProps.data.category.name }} </small>
                </template>
            </Column>
            <Column field="category.name" header="Category"></Column>
            <Column field="target_sum" sortable  header="Целевая сумма тг">
                <template #body="slotProps">
                    {{ slotProps.data.target_sum }}/{{ get_invest_sum(slotProps.data.invests) }}
                    <div hidden>{{ slotProps.data.percent =  get_procent(slotProps.data.target_sum, get_invest_sum(slotProps.data.invests))}}</div>
                    <Slider v-model="slotProps.data.percent" class="w-14rem" deactivate />

                </template>
            </Column>
            <Column field="min_sum" sortable  header="Дапозон вкладов тг">
                <template #body="slotProps">
                    {{ slotProps.data.min_sum }} -  {{ slotProps.data.max_sum }}                
                </template>
            </Column>
            <Column field="min_sum" sortable  header="Даты">
                <template #body="slotProps">
                    С {{ formate_date(slotProps.data.begin_date) }} по {{ formate_date(slotProps.data.end_date) }} 
                </template>
            </Column>
            <Column field="rating" sortable  header="Reviews">
                <template #body="slotProps">
                    <Rating :modelValue="slotProps.data.raiting" readonly :cancel="false" />
                </template>
            </Column>
            <template #footer> In total there are {{ idea_list ? idea_list.length : 0 }} idea_list. </template>
        </DataTable>

    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: 'app',
        data() {
            return {
                category_list: [],
                category_select: null,

                idea_list: []
            };
        },
        methods:{
            async get_category_list(){
                try {
                    const info = await axios.get('http://127.0.0.1:8000/blog/categories/', {Authorization: 'Basic YWRtaW46YWRtaW4='});
                    this.category_list = info.data
                    console.log(">>> start test")
                    console.log(this.category_list);
                } catch (error) {
                    console.error(error);
                }
            },
            async get_ideas_list(){
                try {
                    const info = await axios.get('http://127.0.0.1:8000/blog/idea/', {Authorization: 'Basic YWRtaW46YWRtaW4='}, {category: '3'});
                    this.idea_list = info.data
                    console.log(">>> start test")
                    console.log(this.idea_list);
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
            this.get_category_list();
            this.get_ideas_list();
        }
    }
</script>