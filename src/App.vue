<template>
    <div>
        <nav class="navbar navbar-expand-lg navbar-light bg-dark">
            <div class="container">
                <a class="navbar-brand text-white">Unit converter</a>
            </div>
        </nav>
        <div class="container mt-3">
            <div v-if="choices.length > 0" class="row">
                <div class="col-6">
                    <div class="card text-center">
                        <div class="card-header">
                            From
                        </div>
                        <div class="card-body">
                            <p class="card-title"><input v-model="n" placeholder="choose number"></p>
                        </div>

                        <p class="card-footer">
                            <select v-model="i">
                                <option v-bind:value="choice" v-for="choice in choices" :key="choice.name">
                                    {{choice.name}}
                                </option>
                            </select>
                        </p>
                    </div>
                </div>
                <div class="col-6 text-dark">
                    <div class="card text-center">
                        <div class="card-header">
                            To
                        </div>
                        <div class="card-body">
                            <p v-if="isNaN(n)">{{'"'+ n + '"is not a number'}}</p>
                            <ul v-else class="list-group ">
                                <li class="list-group-item" v-for="choice in choices" :key="choice.name">{{choice.div *
                                    n / i.div }} {{choice.name}}
                                </li>
                            </ul>
                        </div>
                        <div class="card-footer">
                            <button class="btn btn-success" type="button" data-toggle="modal" data-target="#AddForm"
                                    aria-expanded="false" aria-controls="AddForm">
                                Add another unit
                            </button>
                        </div>
                    </div>
                </div>
            </div>
            <div class="row" v-else>
                <div class="card">
                    <div class="card-header">
                        Base unit
                    </div>
                    <div class="card-body">
                        <form>
                            <div class="form-group">
                                <label for="operation-name" class="col-form-label">Base unit name :</label>
                                <input v-model="base.name" type="text" class="form-control" id="operation-nameg">
                            </div>
                        </form>
                    </div>
                    <div class="card-footer ">
                        <button type="button" @click="defineBase" class="btn btn-primary">Define base unit</button>
                    </div>
                </div>
            </div>

            <div class="modal fade" role="dialog" tabindex="-1" aria-hidden="true" id="AddForm">
                <div class="modal-dialog" role="document">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="ModalLabel">new unit specificities</h5>
                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                <span aria-hidden="true">&times;</span>
                            </button>
                        </div>
                        <div class="modal-body">
                            <form>
                                <div class="form-group">
                                    <label for="operation-name" class="col-form-label">Unit name :</label>
                                    <input v-model="unit.name" type="text" class="form-control" id="operation-name">
                                </div>
                                <div class="form-group">
                                    <label for="operation-div" class="col-form-label">Base multiplier (number type):</label>
                                    <input v-model="unit.div" type="text" class="form-control" id="operation-div">
                                </div>

                                <div class="input-group">
                                    <div class="input-group-append">
                                        Example : 1 m = 1 * {{operation}} = {{addExample}} {{unit.name}} or
                                        {{unit.name}}/{{unit.div}}
                                    </div>
                                </div>
                            </form>

                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                            <button type="button" class="btn btn-success" @click="AddUnit" data-dismiss="modal">Add
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                unit: {},
                n: 0,
                base: {div: 1},
                choices: [],
                i: {},
                res: {},

            };
        },
        methods: {
            defineBase: function () {
                let b = this.base
                this.choices.push(b)
                this.i = {...this.choices[0]}
                this.res = {...this.choices[0]}
                this.unit = {...this.choices[0]}
            },
            AddUnit: function () {
                this.choices.push(this.unit)
                this.unit = {...this.choices[0]}
            },

        },
        computed: {
            operation: function () {
                return isNaN(this.unit.div) ? "base multiplier" : this.unit.div.toString()
            },
            addExample: function () {
                let result = 1 * this.unit.div
                return isNaN(result) ? '##' : result.toString()
            },
        },
    };
</script>
