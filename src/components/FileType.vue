<template>
    <div>
        <div v-for="file in bus.project.FileTypeManager.list" :key="file.name">
            <template v-if="file.name==bus.type">
                <table class="table table-striped">
                    <caption class="title">
                        <h1>{{ file.name }}</h1>
                        <span v-if="!file.original" @click="remove(file)" class="btn btn-danger"> X </span>
                    </caption>
                    <thead>
                        <tr>
                            <th width="160px">Name</th>
                            <th>Value</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>path</td>
                            <td>
                                <span @click="change('path', file)" class="btn btn-default">{{ plus(file.path) }}</span>
                            </td>
                        </tr>
                        <tr>
                            <td>NameSpace</td>
                            <td>
                                <span @click="change('NameSpace', file)" class="btn btn-default">{{ plus(file.NameSpace) }}</span>
                            </td>
                        </tr>
                        <tr>
                            <td>prefix</td>
                            <td>
                                <span @click="change('prefix', file)" class="btn btn-default">{{ plus(file.prefix) }}</span>
                            </td>
                        </tr>
                        <tr>
                            <td>suffix</td>
                            <td>
                                <span @click="change('suffix', file)" class="btn btn-default">{{ plus(file.suffix) }}</span>
                            </td>
                        </tr>
                        <tr>
                            <td>templateName</td>
                            <td>
                                <span @click="change('templateName', file)" class="btn btn-default">{{ plus(file.templateName) }}</span>
                            </td>
                        </tr>
                        <tr>
                            <td>template</td>
                            <td>
                                <span @click="visible=!visible" class="btn btn-default"> * </span>
                            </td>
                        </tr>
                        <tr v-show="visible">
                            <td></td>
                            <td><textarea v-model="file.template" rows="11" class="form-control"></textarea></td>
                        </tr>
                    </tbody>
                </table>

                <Property :manager="file.PropertyManager"></Property>
            </template>
        </div>
    </div>
</template>

<script>
    import bus from '../helper/event';
    import { see, sure, enter } from '../helper/dialogue';
    import Property from './Property';

    export default {
        name: 'FileType',
        components: { Property },
        data() {
            return {
                bus,
                visible: false
            };
        },
        methods: {
            plus(key) {
                if (key) {
                    return key;
                }
                return '+';
            },
            change(key, owner) {
                enter('Please enter the ' + key, owner[key]).then(result => {
                    if (result.value === undefined) {
                        return;
                    }
                    try {
                        owner[key] = result.value;
                    } catch (error) {
                        see(error, 400);
                    }
                });
            },
            remove(file) {
                sure('Are you sure?').then(result => {
                    if (result.value) {
                        bus.project.FileTypeManager.remove(file);
                    }
                });
            }
        }
    };
</script>
