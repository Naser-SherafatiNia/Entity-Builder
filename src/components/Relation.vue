<template>
    <tr>
        <td class="button-cell">
            <div class="btn-group">
                <span @click="$emit('remove', relation)" class="btn btn-danger"> X </span>
                <span @click="manager.moveUp(relation)" class="btn btn-default"> ↑ </span>
                <span @click="manager.moveDown(relation)" class="btn btn-default"> ↓ </span>
            </div>
        </td>
        <td>
            <select v-model="relation.type" class="form-control">
                <option value="belongsTo">belongsTo</option>
                <option value="belongsToMany">belongsToMany</option>
                <option value="hasOne">hasOne</option>
                <option value="hasMany">hasMany</option>
            </select>
        </td>
        <td>{{ relation.model }}</td>
        <td>
            <span @click="rename" class="btn btn-default">{{ relation.name }}</span>
        </td>
        <td>
            <span v-if="relation.type == 'belongsToMany'" @click="setPivot" class="btn btn-default">{{ plus(relation.pivot) }}</span>
        </td>
        <td>
            <span v-if="hasForeignKey" @click="setForeignKey" class="btn btn-default">{{ plus(relation.foreignKey) }}</span>
        </td>
        <td>
            <span v-if="hasLocalKey" @click="setLocalKey" class="btn btn-default">{{ plus(relation.localKey) }}</span>
        </td>
    </tr>
</template>

<script>
    import bus from '../helper/event';
    import { see, enter } from '../helper/dialogue';
    import { LDData } from './ListDialogue';

    export default {
        name: 'Relation',
        props: ['relation', 'manager'],
        data() {
            return {
                bus
            };
        },
        computed: {
            hasForeignKey() {
                return this.relation.type != 'belongsTo';
            },
            hasLocalKey() {
                return this.relation.type == 'belongsTo' || this.relation.type == 'belongsToMany';
            }
        },
        methods: {
            plus(key) {
                if (key) {
                    return key;
                }
                return '+';
            },
            rename() {
                enter('Please enter the Method name', this.relation.name).then(result => {
                    if (result.value) {
                        try {
                            this.relation.name = result.value;
                        } catch (error) {
                            see(error, 400);
                        }
                    }
                });
            },
            setPivot() {
                const item = {
                    name: ''
                };
                const list = [item].concat(bus.project.EntityManager.list);
                LDData.show('Select a Table', list, 'name', null, entity => {
                    try {
                        this.relation.pivot = entity.tableName;
                    } catch (error) {
                        see(error, 400);
                    }
                });
            },
            setForeignKey() {
                const key = 'foreignKey';
                if (this.relation.type == 'belongsToMany') {
                    this.setKeyInPivot(key);
                    return;
                }
                const entity = bus.project.EntityManager.findByModel(this.relation.model);
                this.setKey(key, entity);
            },
            setLocalKey() {
                const key = 'localKey';
                if (this.relation.type == 'belongsToMany') {
                    this.setKeyInPivot(key);
                    return;
                }
                const entity = bus.entity;
                this.setKey(key, entity);
            },
            setKeyInPivot(key) {
                if (this.relation.pivot) {
                    const entity = bus.project.EntityManager.findByTable(this.relation.pivot);
                    this.setKey(key, entity);
                    return;
                }
                see('Select a pivot table first!');
            },
            setKey(key, entity) {
                const item = {
                    name: ''
                };
                const list = [item].concat(entity.FieldManager.list);
                LDData.show('Select a Field', list, 'name', null, field => {
                    try {
                        this.relation[key] = field.name;
                    } catch (error) {
                        see(error, 400);
                    }
                });
            }
        }
    };
</script>
