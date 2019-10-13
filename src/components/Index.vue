<template>
    <table class="table">
        <caption>
            <h3>Index</h3>
        </caption>
        <thead>
            <tr>
                <th width="130px"></th>
                <th width="120px">Type</th>
                <th>Field</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="index in manager.list" :key="index.name">
                <td class="button-cell">
                    <div class="btn-group">
                        <span v-on:click="remove(index)" class="btn btn-danger"> X </span>
                        <span v-on:click="manager.moveUp(index)" class="btn btn-default"> ↑ </span>
                        <span v-on:click="manager.moveDown(index)" class="btn btn-default"> ↓ </span>
                    </div>
                </td>
                <td>
                    <select v-model="index.type" class="form-control">
                        <option value="primary">primary</option>
                        <option value="unique">unique</option>
                        <option value="index">index</option>
                    </select>
                </td>
                <td>
                    <span v-for="field in index.FieldManager.list" :key="field.name" class="index-border pull-left">
                        <div class="text-center">{{ field.name }}</div>
                        <div class="btn-group">
                            <span v-on:click="index.FieldManager.moveUp(field)" class="btn btn-info btn-xs"> ← </span>
                            <span v-on:click="index.FieldManager.moveDown(field)" class="btn btn-info btn-xs"> → </span>
                            <span v-on:click="index.FieldManager.remove(field)" class="btn btn-danger btn-xs"> X </span>
                        </div>
                    </span>

                    <span v-on:click="addField(index)" class="btn btn-default"> + </span>
                </td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td></td>
                <td>
                    <span v-on:click="add" class="btn btn-primary plus"> + </span>
                </td>
                <td></td>
            </tr>
        </tfoot>
    </table>
</template>

<script>
    import bus from '../helper/event';
    import { see, sure } from '../helper/dialogue';
    import { LDData } from './ListDialogue';

    export default {
        name: 'Index',
        props: ['manager'],
        data() {
            return {
                bus
            };
        },
        methods: {
            add() {
                const name = 'index' + Date.now();
                const index = this.manager.make(name);
                this.manager.add(index);
            },
            addField(index) {
                LDData.show('Select a Field', bus.entity.FieldManager.list, 'name', null, field => {
                    try {
                        const fff = index.FieldManager.make(field.name);
                        index.FieldManager.add(fff);
                    } catch (error) {
                        see(error, 400);
                    }
                });
            },
            remove(index) {
                sure('Are you sure?').then(result => {
                    if (result.value) {
                        this.manager.remove(index);
                    }
                });
            }
        }
    };
</script>

<style>
    .index-border {
        border: 1px solid darkgray;
        border-radius: 3px;
        margin-right: 4px;
    }
</style>
