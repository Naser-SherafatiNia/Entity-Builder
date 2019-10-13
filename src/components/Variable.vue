<template>
    <table class="table">
        <caption>
            <h3>Variable</h3>
        </caption>
        <thead>
            <tr>
                <th width="130px"></th>
                <th width="100px">Static</th>
                <th width="160px">Name</th>
                <th>Value</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="variable in manager.list" :key="variable.name">
                <td>
                    <div class="btn-group">
                        <span v-on:click="remove(variable)" class="btn btn-danger"> X </span>
                        <span v-on:click="manager.moveUp(variable)" class="btn btn-default"> ↑ </span>
                        <span v-on:click="manager.moveDown(variable)" class="btn btn-default"> ↓ </span>
                    </div>
                </td>
                <td>
                    <label><input v-model="variable.isStatic" class="checkbox" type="checkbox"/></label>
                </td>
                <td>
                    <span v-on:click="rename(variable)" class="btn btn-default">${{ variable.name }}</span>
                </td>
                <td>
                    <input v-model="variable.value" class="form-control" type="text" />
                </td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td></td>
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
    import { see, sure, enter } from '../helper/dialogue';

    export default {
        name: 'Variable',
        props: ['manager'],
        data() {
            return {};
        },
        methods: {
            add() {
                enter('Please enter the Variable name').then(result => {
                    if (result.value) {
                        try {
                            const variable = this.manager.make(result.value);
                            this.manager.add(variable);
                        } catch (error) {
                            see(error, 400);
                        }
                    }
                });
            },
            rename(variable) {
                enter('Please enter the Variable name', variable.name).then(result => {
                    if (result.value) {
                        try {
                            variable.name = result.value;
                        } catch (error) {
                            see(error, 400);
                        }
                    }
                });
            },
            remove(variable) {
                sure('Are you sure?').then(result => {
                    if (result.value) {
                        this.manager.remove(variable);
                    }
                });
            }
        }
    };
</script>
