<template>
    <div v-if="show">
        <vue-bpmn @save="btnSave" :modelData="modelData" :isView="isView"/>
    </div>
</template>
<script>
    import {getAction, putAction} from '@/api/manage'
    import {Message} from 'element-ui'
    // import VueBpmn from "@/components/VueBpmn"; // 原生面板
    // import VueBpmn from "@/components/VueElementuiBpmn"; // elementui面板
    import VueBpmn from "@/components/VueBpmn";

    export default {
        name: 'FlowableModelEdit',
        components: {VueBpmn},
        data() {
            return {
                id: undefined,
                isView: false,
                modelData: {
                    id: undefined,
                    editor: undefined
                },
                show: false
            }
        },
        mounted() {
            if (this.$route.query && this.$route.query.id) {
                this.id = this.$route.query.id
            }
            if (this.$route.query && this.$route.query.isView) {
                this.isView = this.$route.query.isView
            }
            this.getModelData()
        },
        methods: {
            getModelData() {
                if (!this.id) {
                    Message.error('id is null')
                    return
                }
                getAction('/flowable/model/queryById', {id: this.id}).then(({data}) => {
                    this.modelData.id = data.id
                    this.modelData.editor = data.editor
                    this.modelData.key = data.key
                    this.modelData.name = data.name
                    this.modelData.category = data.category
                    this.modelData.description = data.description
                    this.show = true
                })
            },
            btnSave(modelData) {
                putAction('/flowable/model/saveModelEditor', modelData).then(({msg, data}) => {
                    Message.success(msg)
                })
            }
        }
    }
</script>