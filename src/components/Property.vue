<template>
    <table class="table">
        <caption>
            <h3>
                <slot>Property</slot>
            </h3>
        </caption>
        <thead>
            <tr>
                <th width="130px"></th>
                <th width="160px">Name</th>
                <th>Value</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="property in manager.list" :key="property.name">
                <td class="button-cell">
                    <div class="btn-group">
                        <span v-on:click="remove(property)" class="btn btn-danger"> X </span>
                        <span v-on:click="manager.moveUp(property)" class="btn btn-default"> ↑ </span>
                        <span v-on:click="manager.moveDown(property)" class="btn btn-default"> ↓ </span>
                    </div>
                </td>
                <td>
                    <span @click="setName(property)" class="btn btn-default">{{ property.name }}</span>
                </td>
                <td>
                    <span @click="setValue(property)" class="btn btn-default">{{ plus(property.value) }}</span>
                </td>
            </tr>
            <tr>
                <td></td>
                <td>
                    <span v-if="!noadd" @click="add" class="btn btn-primary plus"> + </span>
                </td>
                <td></td>
            </tr>
        </tbody>
    </table>
</template>

<script>
    import { see, enter, sure } from '../helper/dialogue';

    export default {
        name: 'Property',
        props: ['manager', 'noadd'],
        data() {
            return {};
        },
        methods: {
            plus(key) {
                if (key) {
                    return key;
                }
                return '+';
            },
            add() {
                enter('Please enter the name').then(result => {
                    if (result.value) {
                        try {
                            const property = this.manager.make(result.value);
                            this.manager.add(property);
                        } catch (error) {
                            see(error, 400);
                        }
                    }
                });
            },
            remove(property) {
                sure('Are you sure?').then(result => {
                    if (result.value) {
                        this.manager.remove(property);
                    }
                });
            },
            setName(property) {
                enter('Please enter the name', property.name).then(result => {
                    if (result.value) {
                        try {
                            property.name = result.value;
                        } catch (error) {
                            see(error, 400);
                        }
                    }
                });
            },
            setValue(property) {
                enter('Please enter the value', property.value).then(result => {
                    if (result.value) {
                        try {
                            property.value = result.value;
                        } catch (error) {
                            see(error, 400);
                        }
                    }
                });
            }
        }
    };
</script>
