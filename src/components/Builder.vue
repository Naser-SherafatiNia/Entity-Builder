<template>
    <div>
        <div v-if="'project' == bus.tab" class="title">
            <h1>Project</h1>
            <span class="btn-group">
                <span v-on:click="make" class="btn btn-primary">New</span>
                <label class="btn btn-info">
                    Load
                    <input v-on:change="load($event)" type="file" accept=".json" style="display:none;" />
                </label>
                <span @click="download" :disabled="bus.project==null" class="btn btn-success">Download</span>
                <span v-if="bus.php" @click="save" :disabled="bus.project==null" class="btn btn-success">Save</span>
                <span v-else @click="connect" class="btn btn-success">Connect</span>
            </span>
            <span v-if="bus.project">{{ bus.project.fileName }}</span>
        </div>

        <Project v-if="bus.project"></Project>
    </div>
</template>

<script>
    import { addUser, loadProject, makeProject } from '../helper/entity';
    import { see, sure, enter } from '../helper/dialogue';
    import bus from '../helper/event';
    import { read } from '../helper/net';
    import { connect, getDB, save } from '../helper/request';
    import { download } from '../helper/zip';
    import Project from './Project';

    export default {
        name: 'Builder',
        components: { Project },
        data() {
            return {
                bus
            };
        },
        methods: {
            connect() {
                enter('Please enter the domain', 'http://localhost').then(result => {
                    if (result.value) {
                        connect(result.value)
                            .then(response => {
                                try {
                                    // console.log(response)
                                    bus.php = true;
                                    if (bus.project) {
                                        return;
                                    }
                                    if (response.data) {
                                        const json = JSON.parse(response.data);
                                        bus.project = loadProject(json);
                                        if (bus.project.EntityManager.list.length > 0) {
                                            bus.show(bus.project.EntityManager.list[0]);
                                        }
                                    } else {
                                        if (bus.project == null) {
                                            sure('Do you want to load tables from your database schema?')
                                                .then(result => {
                                                    if (result.value) {
                                                        this.convert();
                                                    }
                                                })
                                                .catch(error => {
                                                    console.log(error);
                                                });
                                        }
                                    }
                                } catch (error) {
                                    see(error, 400);
                                }
                            })
                            .catch(error => {
                                see(error.message, 400);
                            });
                    }
                });
            },
            convert() {
                getDB()
                    .then(response => {
                        if (response.data) {
                            bus.project = makeProject('entity');
                            bus.project.convertDB(response.data);
                            if (bus.project.EntityManager.list.length > 0) {
                                bus.show(bus.project.EntityManager.list[0]);
                            }
                        }
                    })
                    .catch(error => {
                        see(error.message, 400);
                    });
            },
            save() {
                if (bus.project == null) {
                    return;
                }

                save(bus.project)
                    .then(response => {
                        see(response.message, 200);
                    })
                    .catch(error => {
                        see(error.message, 400);
                    });
            },
            warning() {
                if (bus.project == null) {
                    return true;
                }

                return confirm('Warning!\nUnsaved changes will be lost!\nContinue?');
            },
            load() {
                if (this.warning()) {
                    // go on
                } else {
                    return;
                }

                const file = event.target.files[0];
                if (file) {
                    read(file, text => {
                        try {
                            const json = JSON.parse(text);
                            bus.project = loadProject(json);
                            if (bus.project.EntityManager.list.length > 0) {
                                bus.show(bus.project.EntityManager.list[0]);
                            }
                        } catch (error) {
                            see(error, 400);
                        }
                    });
                }
            },
            make() {
                if (this.warning()) {
                    // go on
                } else {
                    return;
                }

                enter('Please enter the Project name', 'entity').then(result => {
                    if (result.value) {
                        try {
                            bus.project = makeProject(result.value);
                            const user = addUser(bus.project);
                            bus.show(user);
                        } catch (error) {
                            see(error, 400);
                        }
                    }
                });
            },
            download() {
                if (bus.project == null) {
                    return;
                }

                try {
                    save(bus.project);
                } catch (error) {
                    see(error, 400);
                }
            }
        }
    };
</script>
