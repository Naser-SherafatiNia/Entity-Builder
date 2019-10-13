<template>
    <table class="table">
        <caption>
            <h3>Faker</h3>
        </caption>
        <thead>
            <tr>
                <th width="120px">Field</th>
                <th width="120px">Type</th>
                <th>Method / Property</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="field in manager.list" :key="field.name">
                <td>{{ field.name }}</td>
                <td>
                    <select v-model="field.seed.type" class="form-control">
                        <option value="raw">Raw</option>
                        <option value="property">Property</option>
                        <option value="method">Method</option>
                    </select>
                </td>
                <td>
                    <div v-if="'raw' == field.seed.type">
                        <input v-model="field.seed.raw" class="form-control" type="text" />
                    </div>
                    <div v-else class="form-inline">
                        <label><input v-model="field.seed.unique" type="checkbox" /> Unique</label>

                        <template v-if="'method' == field.seed.type">
                            <span v-on:click="setMethod(field)" class="btn btn-default">{{ field.seed.method }}</span>
                            (
                            <input v-model="field.seed.parameter" class="form-control" type="text" />
                            )
                        </template>

                        <template v-if="'property' == field.seed.type">
                            <span v-on:click="setProperty(field)" class="btn btn-default">{{ field.seed.property }}</span>
                        </template>
                    </div>
                </td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td></td>
                <td></td>
                <td></td>
            </tr>
        </tfoot>
    </table>
</template>

<script>
    import Entity from '../helper/entity';
    import { LDData } from './ListDialogue';

    export default {
        name: 'Faker',
        props: ['manager'],
        data() {
            return {};
        },
        created() {},
        methods: {
            setMethod(field) {
                LDData.show('Select a Method', Entity.FakerMethodList, null, null, result => {
                    field.seed.method = result;
                });
            },
            setProperty(field) {
                LDData.show('Select a Property', Entity.FakerPropertyList, null, null, result => {
                    field.seed.property = result;
                });
            }
        }
    };
</script>
