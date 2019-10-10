<template>
    <table class="table table-striped">
        <caption>
            <h3>Entity</h3>
        </caption>
        <thead>
            <tr>
                <th width="200px"></th>
                <th>Name</th>
                <th>Table</th>
                <th>File</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="entity in manager.list" :key="entity.name">
                <td class="button-cell">
                    <div class="btn-group">
                        <span @click="remove(entity)" class="btn btn-danger"> X </span>
                        <span @click="manager.moveUp(entity)" class="btn btn-default"> ↑ </span>
                        <span @click="manager.moveDown(entity)" class="btn btn-default"> ↓ </span>
                        <span v-if="bus.php" @click="deployGroup(entity)" class="btn btn-success">Deploy</span>
                        <span v-else @click="zip(entity)" class="btn btn-success">Zip</span>
                    </div>
                </td>
                <td>
                    <span @click="setName(entity)" class="btn btn-default">{{ entity.name }}</span>
                </td>
                <td>
                    <span @click="setTableName(entity)" class="btn btn-default">{{ entity.tableName }}</span>
                </td>
                <td>
                    <div class="btn-group">
                        <span v-for="file in entity.FileManager.list" @click="deploy(file, entity)" :key="file.fileTypeName" class="btn btn-default btn-sm">
                            {{ file.fileTypeName }}
                        </span>
                    </div>
                </td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td>
                    <span v-if="bus.php" @click="deployAll" class="btn btn-success">Deploy</span>
                    <span v-else @click="zipAll" class="btn btn-success">Zip All</span>
                </td>
                <td></td>
                <td></td>
                <td></td>
            </tr>
        </tfoot>
    </table>
</template>

<script>
    import bus from '../helper/event';
    import { see, sure, enter } from '../helper/dialogue';
    import { deployFile, deployGroup, deployAll } from '../helper/request';
    import { zipAll, zipEntity } from '../helper/zip';

    export default {
        name: 'Entity',
        props: ['manager'],
        data() {
            return {
                bus
            };
        },
        methods: {
            setName(entity) {
                enter('Please enter the Main name', entity.name).then(result => {
                    if (result.value) {
                        try {
                            entity.name = result.value;
                        } catch (error) {
                            see(error, 400);
                        }
                    }
                });
            },
            setTableName(entity) {
                enter('Please enter the Table name', entity.tableName).then(result => {
                    if (result.value) {
                        try {
                            entity.tableName = result.value;
                        } catch (error) {
                            see(error, 400);
                        }
                    }
                });
            },
            remove(entity) {
                sure('Are you sure?').then(result => {
                    if (result.value) {
                        this.manager.remove(entity);
                    }
                });
            },
            deploy(file, entity) {
                if (bus.php) {
                    deployFile(bus.project, entity, file)
                        .then(response => {
                            see(response.data.message, 200);
                        })
                        .catch(error => {
                            see(error.message, 400);
                        });
                }
            },
            deployGroup(entity) {
                deployGroup(bus.project, entity)
                    .then(response => {
                        see(response.data.message, 200);
                    })
                    .catch(error => {
                        see(error.message, 400);
                    });
            },
            deployAll() {
                deployAll(bus.project)
                    .then(response => {
                        see(response.data.message, 200);
                    })
                    .catch(error => {
                        see(error.message, 400);
                    });
            },
            zip(entity) {
                try {
                    zipEntity(entity, bus.project);
                } catch (error) {
                    see(error, 400);
                }
            },
            zipAll() {
                try {
                    zipAll(bus.project);
                } catch (error) {
                    see(error, 400);
                }
            }
        }
    };
</script>
