<template>
    <div>
        <ul @click="editing=true" class="rule">
            <li v-for="rule in manager.list" :key="rule.name">
                <Rule :rule="rule" :editing="editing" @remove="remove(rule)"></Rule>
            </li>
        </ul>

        <div v-if="editing">
            <br>
            <ul class="nav nav-tabs">
                <li>
                    <span @click="editing=false" class="btn btn-primary">OK</span>&nbsp;
                </li>
                <li v-for="kind in KindList" :key="kind.title" role="presentation" :class="{active:kind.title==tab}">
                    <a @click="tab=kind.title" href="javascript:void(0)">{{ kind.title }}</a>
                </li>
            </ul>
            <ul v-for="kind in kindInTab" :key="kind.title" class="rule">
                <li v-for="rule in kind.list" :key="rule.name">
                    <span @click="add(rule)" class="btn btn-default"> + </span>
                    <a :href="link(rule)" target="_blank">{{ rule.name }}</a>
                </li>
            </ul>
        </div>

        <div v-else class="none-rule">
            <div v-if="manager.list.length==0" @click="editing=true">...</div>
        </div>
    </div>
</template>

<script>
    import { see, sure } from '../helper/dialogue';
    import { KindList, REList } from '../helper/rule';
    import Rule from './Rule';

    export default {
        name: 'RuleTab',
        components: { Rule },
        props: ['manager'],
        data() {
            return {
                REList,
                KindList,
                editing: false,
                tab: 'Content'
            };
        },
        computed: {
            kindInTab() {
                return this.KindList.filter(kind => kind.title == this.tab);
            }
        },
        methods: {
            add(rule) {
                try {
                    const rrr = this.manager.make(rule.name, rule.isBoolean);
                    this.manager.add(rrr);
                } catch (error) {
                    see(error, 400);
                }
            },
            remove(rule) {
                sure('Are you sure?').then(result => {
                    if (result.value) {
                        this.manager.remove(rule);
                    }
                });
            },
            link(rule) {
                const uri = 'https://laravel.com/docs/6.0/validation';
                if (rule.name === 'sometimes') {
                    return uri + '#conditionally-adding-rules';
                }
                const name = rule.name.replace('_', '-');
                return `${uri}#rule-${name}`;
            }
        }
    };
</script>
