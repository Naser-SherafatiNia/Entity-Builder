<template>
    <div @click="editing=true">
        <div v-if="editing">
            <span>Default: </span>
            <span class="input-group">
                <span class="input-group-addon">
                    <input type="checkbox" @click="check(field)" :checked="field.value" />
                </span>
                <input v-model="field.value" class="form-control" type="text" />
            </span>
        </div>
        <div v-else>
            <span v-if="field.value">Default: {{ field.value }}</span>
        </div>

        <div v-if="editing">
            <span>Comment: </span>
            <input v-model="field.comment" class="form-control" type="text" />
        </div>
        <div v-else>
            <span v-if="field.comment">Comment: {{ field.comment }}</span>
        </div>

        <div v-if="editing">
            <label><input v-model="field.allowNull" class="checkbox" type="checkbox" /> Allow Null</label>
        </div>
        <div v-else>
            <span v-if="field.allowNull">Allow Null</span>
        </div>

        <template v-if="field.isNumber">
            <div v-if="editing">
                <label><input v-model="field.unsigned" class="checkbox" type="checkbox" /> Unsigned</label>
            </div>
            <div v-else>
                <span v-if="field.unsigned">Unsigned</span>
            </div>
        </template>

        <div v-if="editing">
            <span @click.stop="editing=false" class="btn btn-primary">OK</span>
        </div>
        <div v-else>...</div>
    </div>
</template>

<script>
    export default {
        name: 'FieldPanel',
        props: ['field'],
        data() {
            return {
                editing: false
            };
        },
        methods: {
            check(field) {
                if (field.value === '') {
                    if (field.isNumber) {
                        field.value = '0';
                    } else {
                        field.value = "''";
                    }
                } else {
                    field.value = '';
                }
            }
        }
    };
</script>
