<template>
    <table class="table">
        <caption>
            <h3>Relation</h3>
        </caption>
        <thead>
            <tr>
                <th width="130px"></th>
                <th width="180px">Type</th>
                <th>Model</th>
                <th>Method Name</th>
                <th>Pivot Table</th>
                <th>Foreign Field</th>
                <th>Local Field</th>
            </tr>
        </thead>
        <tbody>
            <Relation v-for="relation in manager.list" :relation="relation" :manager="manager" :key="relation.name" @remove="remove"></Relation>
        </tbody>
        <tfoot>
            <tr>
                <td></td>
                <td>
                    <select @change="add($event.target.value)" class="form-control">
                        <option selected="true" disabled="disabled">...</option>
                        <option v-for="entity in EntityList" :value="entity.name" :key="entity.name">{{ entity.name }}</option>
                    </select>
                </td>
                <td></td>
                <td></td>
                <td></td>
                <td></td>
                <td></td>
            </tr>
        </tfoot>
    </table>
</template>

<script>
    import bus from '../helper/event';
    import { see, sure } from '../helper/dialogue';
    import Relation from './Relation';

    export default {
        name: 'RelationList',
        components: { Relation },
        props: ['manager'],
        data() {
            return {
                bus,
                EntityList: bus.project.EntityManager.list
            };
        },
        created() {},
        methods: {
            add(name) {
                try {
                    const found = this.EntityList.find(item => item.name === name);
                    const relation = this.manager.link(found);
                    this.manager.add(relation);
                } catch (error) {
                    see(error, 400);
                }
            },
            remove(relation) {
                sure('Are you sure?').then(result => {
                    if (result.value) {
                        this.manager.remove(relation);
                    }
                });
            }
        }
    };
</script>
