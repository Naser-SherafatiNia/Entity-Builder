<template>
    <table class="table table-striped">
        <caption>
            <h3>Cast</h3>
        </caption>
        <thead>
            <tr>
                <th width="120px">Field</th>
                <th width="120px">Type</th>
                <th width="66px">Fillable</th>
                <th width="66px">Hidden</th>
                <th>Cast</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="field in manager.list" :key="field.name">
                <td>{{ field.name }}</td>
                <td>{{ field.type }}</td>
                <td><input v-model="field.fillable" class="checkbox" type="checkbox" /></td>
                <td><input v-model="field.hidden" class="checkbox" type="checkbox" /></td>
                <td>
                    <div class="btn-group">
                        <span v-on:click="select(field)" class="btn btn-default">{{ plus(field) }}</span>
                        <span v-on:click="change(field)" class="btn btn-default"> * </span>
                    </div>
                </td>
            </tr>
        </tbody>
    </table>
</template>

<script>
    import { enter } from '../helper/dialogue';
    import { LDData } from './ListDialogue';

    const CastTypeList = ['', 'array', 'boolean', 'collection', 'date', 'datetime', 'decimal:8', 'double', 'float', 'integer', 'object', 'real', 'string', 'timestamp'];

    export default {
        name: 'Cast',
        props: ['manager'],
        data() {
            return {};
        },
        methods: {
            plus(field) {
                if (field.cast) {
                    return field.cast;
                }
                return '+';
            },
            select(field) {
                LDData.show('Select a Type', CastTypeList, null, null, resule => {
                    field.cast = resule;
                });
            },
            change(field) {
                if (field.cast) {
                    enter('Please enter something', field.cast).then(result => {
                        if (result.value) {
                            field.cast = result.value;
                        }
                    });
                }
            }
        }
    };
</script>
