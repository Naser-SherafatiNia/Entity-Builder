<template>
    <span :class="{editing:editing}">
        <template v-if="editing">
            <span @click="$emit('remove')" class="btn btn-default">- {{ rule.name }}</span>&nbsp;

            <template v-if="rule.isBoolean===false">
                <span v-if="isRE" class="input-group">
                    <span class="input-group-btn">
                        <span class="btn btn-default dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false"> * </span>
                        <ul class="dropdown-menu">
                            <template v-for="(re, index) in REList">
                                <li v-if="re.name" :key="index">
                                    <a @click="rule.value = re.text" href="javascript:void(0)">{{ re.name }}</a>
                                </li>
                                <li v-else :key="index" role="separator" class="divider"></li>
                            </template>
                        </ul>
                    </span>
                    <input v-model="rule.value" class="form-control" type="text" />
                </span>

                <input v-else v-model="rule.value" class="form-control" type="text" />
            </template>
        </template>

        <template v-else>
            <span>{{ rule.name }}</span>
            <span v-if="rule.isBoolean===false"> {{ rule.value }}</span>
        </template>
    </span>
</template>

<script>
    import { REList } from '../helper/rule';

    export default {
        name: 'Rule',
        props: ['rule', 'editing'],
        data() {
            return {
                REList
            };
        },
        computed: {
            isRE() {
                return this.rule.name === 'regex' || this.rule.name === 'not_regex';
            }
        }
    };
</script>
