<template>
    <div>
        <div class="title file-title button-cell">
            <h1>{{ type }}</h1>
            <template v-if="file">
                <span class="btn-group">
                    <span @click="remove" class="btn btn-danger"> X </span>
                    <span @click="template" class="btn btn-primary">Template</span>
                    <span @click="preview" class="btn btn-success">Preview</span>
                </span>
                <span>{{ file.fileName }}</span>
            </template>
            <span v-else @click="add" class="btn btn-primary"> + </span>
        </div>

        <div v-if="file">
            <slot></slot>
        </div>
    </div>
</template>

<script>
    import bus from '../helper/event';
    import { see, sure } from '../helper/dialogue';
    import render from '../helper/render';
    import Template from '../helper/template';
    import { CDData } from './CodeDialogue';

    export default {
        name: 'File',
        props: ['file', 'type'],
        data() {
            return {
                bus
            };
        },
        methods: {
            add() {
                try {
                    const file = bus.entity.FileManager.make(this.type);
                    bus.entity.FileManager.add(file);
                    bus.file = file;
                } catch (error) {
                    see(error, 400);
                }
            },
            template() {
                let template = '';
                const type = this.file.FileType;
                const original = Template[type.templateName];
                if (type.template) {
                    template = type.template;
                } else {
                    if (type.templateName) {
                        template = original;
                    }
                }

                CDData.show(type.name, template, true, original, code => {
                    if (code === original) {
                        type.template = '';
                    } else {
                        type.template = code;
                    }
                });
            },
            preview() {
                try {
                    const code = render(bus.project, bus.entity, this.file);
                    CDData.show(this.file.fileName, code);
                } catch (error) {
                    see(error, 400);
                }
            },
            remove() {
                sure('Are you sure?').then(result => {
                    if (result.value) {
                        bus.entity.FileManager.remove(this.file);
                        bus.showTab('project');
                    }
                });
            }
        }
    };
</script>
