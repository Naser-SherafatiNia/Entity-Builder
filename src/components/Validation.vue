<template>
    <table class="table table-striped">
        <caption>
            <h3>Validation</h3>
        </caption>
        <thead>
            <tr>
                <th width="120px">Field</th>
                <th width="120px">Type</th>
                <th>Rule</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="field in manager.list" :key="field.name" :class="{'disabled':!field.included}">
                <td>
                    <label> <input v-model="field.included" type="checkbox" /> {{ field.name }} </label>
                </td>
                <td>{{ field.type }}</td>
                <td>
                    <RuleTab :manager="field.RuleManager"></RuleTab>
                </td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td colspan="2">
                    <span v-on:click="setRule" class="btn btn-primary">Auto Set Rule</span>
                </td>
            </tr>
        </tfoot>
    </table>
</template>

<script>
    import { check } from '../helper/rule';
    import RuleTab from './RuleTab';

    export default {
        name: 'Validation',
        components: { RuleTab },
        props: ['manager', 'entity'],
        data() {
            return {};
        },
        created() {},
        methods: {
            setRule() {
                check(this.entity);
            }
        }
    };
</script>

<style>
    .table tr.disabled {
        color: #888888;
    }

    ul.rule {
        margin: 0;
        padding: 0;
    }

    .table tr.disabled ul.rule {
        display: none;
    }

    .table tr.disabled .none-rule {
        display: none;
    }

    ul.rule .editing {
        line-height: 3;
    }

    ul.rule input {
        width: 333px;
        display: inline-block;
    }
</style>
