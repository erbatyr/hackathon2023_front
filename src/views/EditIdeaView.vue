<template>
    <div id="app">

        <div class="p-inputgroup flex-1">
            <span class="p-inputgroup-addon">
                <i class="pi pi-user"></i>
            </span>
            <InputText v-model="form_info.label" type="text" size="large" placeholder="Название" />
        </div>

        <div class="p-inputgroup flex-1 ">
            <Textarea v-model="form_info.short_description" rows="5" cols="30" placeholder="Краткое описание"/>
        </div>

        <div class="p-inputgroup flex-1 ">
            <Rating v-model="form_info.raiting" />
        </div>

        <div class="p-inputgroup flex-1 ">
            <Chips v-model="form_info.teg" separator=","  placeholder="Тэги"/>
        </div>
        
        <div class="p-inputgroup flex-1 ">
            <Textarea v-model="form_info.contacts" rows="5" cols="30" placeholder="Контакты"/>
        </div>
        
        
        <div class="p-inputgroup flex-1">
            <span class="p-inputgroup-addon">Target</span>
            <InputNumber v-model="form_info.target_sum" placeholder="Целевая сумма" />
            <span class="p-inputgroup-addon">тг</span>
        </div>
        <div class="p-inputgroup flex-1">
            <span class="p-inputgroup-addon">Min</span>
            <InputNumber v-model="form_info.min_sum" placeholder="Минимальная сумма вклада" />
            <span class="p-inputgroup-addon">тг</span>
            <span class="p-inputgroup-addon">Max</span>
            <InputNumber v-model="form_info.max_sum" placeholder="Максимальная сумма вклада" />
            <span class="p-inputgroup-addon">тг</span>
        </div>
        <div class="p-inputgroup flex-1">
        </div>
        
        <div class="p-inputgroup flex-1 ">
            <Calendar id="calendar-24h" v-model="form_info.begin_date" showTime hourFormat="24" showIcon placeholder="Начало сбора"/>
            <Calendar id="calendar-24h" v-model="form_info.end_date" showTime hourFormat="24" showIcon placeholder="Конец сбора"/>
        </div>

        <Editor v-model="value" editorStyle="height: 320px" />

        




        <div>
            {{form_info}}
        </div>
    </div>
</template>

<script>
    
    export default {
        name: 'app',
        data() {
            return {

                form_info: {
                    owner: 1, // TODO: add current owner
                    label: null,
                    short_description: null,
                    description: null,
                    teg: null,

                    contacts: null,
                    raiting: null,

                    target_sum: null,
                    min_sum: null,
                    max_sum: null,

                    begin_date: null,
                    end_date: null,
                    category: null,

                    status: "open",
                    approve_status: "wait",
                    is_fin_checked: false,
                    is_halyk_checked: false,
                    is_legal_checked: false,
                }
            };
        }
    }
</script>