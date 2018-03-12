<template>
    <div class="container aeris-eccad-datasearch">
        <div>

            <div class="aeris-eccad-datasearch-overviewFlex">

                <div class="aeris-eccad-datasearch-emissionDatatypeClass">
                    <span class="aeris-eccad-datasearch-overviewTitle">{{$t('emissions')}}</span>
                    <span class="aeris-eccad-datasearch-eccadBadge" v-if="selectedData.length >= 1">{{selectedEmissionCategorySize}}</span>
                    <ul class="aeris-eccad-datasearch-overviewListM">
                        <li :class="emissionDatatypeClass(datatype)"
                            v-for="(datatype, index) in uniqueEmissionCategories()"
                            v-on:click="datatypeChanged(datatype)">{{datatype.fullname_category}}
                        </li>
                    </ul>
                </div>

                <div class="aeris-eccad-datasearch-ancillaryDatatypeClass">
                    <span class="aeris-eccad-datasearch-overviewTitle">{{$t('dataancillary')}}</span>
                    <span class="aeris-eccad-datasearch-eccadBadge" v-if="selectedData.length >= 1">{{selectedAncillaryCategorySize}}</span>
                    <ul class="aeris-eccad-datasearch-overviewListM">
                        <li :class="ancillaryDatatypeClass(datatype)"
                            v-for="(datatype, index) in uniqueAncillaryCategories()"
                            v-on:click="datatypeChanged(datatype)">{{datatype.fullname_category}}
                        </li>
                    </ul>
                </div>

                <div class="aeris-eccad-datasearch-speciesClass">
                    <span class="aeris-eccad-datasearch-overviewTitle">{{$t('parameters')}}</span>
                    <span class="aeris-eccad-datasearch-eccadBadge"
                          v-if="selectedData.length >= 1">{{selectedParameterSize}}</span>
                    <ul class="aeris-eccad-datasearch-overviewListML">
                        <li :class="speciesClass(parameter)"
                            v-for="(parameter, index) in uniqueParameters()"

                            v-on:click="speciesChanged(parameter)">{{$t(parameter.display_name_parametre)}}
                        </li>
                    </ul>
                </div>

                <div class="aeris-eccad-datasearch-datasetGroupClass">
                    <span class="aeris-eccad-datasearch-overviewTitle">{{$t('datasets')}}</span>
                    <span class="aeris-eccad-datasearch-eccadBadge"
                          v-if="selectedData.length >= 1">{{selectedGroupOfDatasetSize}}</span>
                    <ul class="aeris-eccad-datasearch-overviewListML">
                        <li :class="datasetGroupClass(inventoryGroup)"
                            v-for="(inventoryGroup, index) in uniqueDatasetGroup()"

                            v-on:click="inventoryGroupChanged(inventoryGroup)">
                            {{$t(inventoryGroup.title_inventory_group)}}
                        </li>
                    </ul>

                </div>

                <div class="aeris-eccad-datasearch-resolutionClass">

                    <span class="aeris-eccad-datasearch-overviewTitle">{{$t('resolutions')}}</span>
                    <span class="aeris-eccad-datasearch-eccadBadge" v-if="selectedData.length >= 1">{{selectedResolutionSize}}</span>
                    <ul class="aeris-eccad-datasearch-overviewListL">
                        <li :class="resolutionClass(resolution)"
                            v-for="(resolution, index) in uniqueResolution()"

                            v-on:click="resolutionChanged(resolution)">{{resolution.fullname_resolution}}
                        </li>
                    </ul>
                </div>

                <div class="aeris-eccad-datasearch-sectorGroupClass">
                    <span class="aeris-eccad-datasearch-overviewTitle">{{$t('groupofsectors')}}</span>
                    <span v-if="selectedAllSectorInventoryGroup.length === 1 && selectedAllSectorInventoryGroup.length !==0 && selectedAllSectorInventoryGroup[0].id_sector === -1"
                          class="aeris-eccad-datasearch-eccadBadge">{{0}}</span>
                    <span v-else class="aeris-eccad-datasearch-eccadBadge">{{selectedGroupOfSectorSize}}</span>
                    <ul class="aeris-eccad-datasearch-overviewListMLW">
                        <li :class="sectorGroupClass(sectorGroup)"
                            v-for="(sectorGroup, index) in uniqueSectorGroup()"
                            v-on:click="sectorGroupChanged(sectorGroup)">{{$t(sectorGroup.name_sector_group)}}
                        </li>
                    </ul>

                </div>

                <div class="aeris-eccad-datasearch-speciesGroupClass">
                    <span class="aeris-eccad-datasearch-overviewTitle">{{$t('groupofparameters')}}</span>
                    <span v-if="selectedGroupOfSpecieSize === 1 && selectedData.length !==0 && selectedData[0].fullname_parametre_group === 'None'"
                          class="aeris-eccad-datasearch-eccadBadge">{{0}}</span>
                    <span v-else class="aeris-eccad-datasearch-eccadBadge">{{selectedGroupOfSpecieSize}}</span>
                    <ul class="aeris-eccad-datasearch-overviewList">
                        <li :class="speciesGroupClass(speciesGroup)"
                            v-for="(speciesGroup, index) in uniqueParameterGroup()"

                            v-on:click="speciesGroupChanged(speciesGroup)">{{$t(speciesGroup.fullname_parametre_group)}}
                        </li>
                    </ul>
                </div>

                <div class="aeris-eccad-datasearch-scenarioClass">
                    <span class="aeris-eccad-datasearch-overviewTitle">{{$t('scenarios')}}</span>
                    <span v-if="selectedScenarioSize === 1 && selectedData.length !==0 && selectedData[0].id_scenario === -1"
                          class="aeris-eccad-datasearch-eccadBadge">{{0}}</span>
                    <span v-else class="aeris-eccad-datasearch-eccadBadge">{{selectedScenarioSize}}</span>
                    <ul class="aeris-eccad-datasearch-overviewList">
                        <li :class="scenarioClass(scenario)"
                            v-for="(scenario, index) in uniqueScenario()"

                            v-on:click="scenarioChanged(scenario)">{{$t(scenario.display_name_scenario)}}
                        </li>
                    </ul>

                </div>

                <div class="aeris-eccad-datasearch-geospatialClass">
                    <span class="aeris-eccad-datasearch-overviewTitle">{{$t('geospatial')}}</span>
                    <span class="aeris-eccad-datasearch-eccadBadge" v-if="selectedData.length >= 1">{{selectedGeospatialSize}}</span>
                    <ul class="aeris-eccad-datasearch-overviewListM">
                        <li :class="geospatialClass(geospatial)"
                            v-for="(geospatial, index) in uniqueGeospatial()"

                            v-on:click="geospatialChanged(geospatial)">{{$t(geospatial.fullname_geospatial)}}
                        </li>
                    </ul>


                </div>


                <div class="aeris-eccad-datasearch-sectorClass">
                    <span class="aeris-eccad-datasearch-overviewTitle">{{$t('sectors')}}</span>
                    <span v-if="selectedAllSectorInventoryGroup.length === 1 && selectedAllSectorInventoryGroup.length !==0 && selectedAllSectorInventoryGroup[0].id_sector === -1"
                          class="aeris-eccad-datasearch-eccadBadge">{{0}}</span>
                    <span v-else class="aeris-eccad-datasearch-eccadBadge">{{selectedSectorSize}}</span>
                    <ul class="aeris-eccad-datasearch-overviewListMLW">
                        <li :class="sectorClass(sector)"
                            v-for="(sector, index) in uniqueSector()"

                            v-on:click="sectorChanged(sector)">{{$t(sector.name_sector)}}
                        </li>
                    </ul>
                </div>

                <div class="aeris-eccad-datasearch-temporalClass">
                    <div class="aeris-eccad-datasearch-sliderClass">
                        <span class="aeris-eccad-datasearch-overviewTitle">{{$t('temporal')}}</span>
                        <vue-slider ref="slider4" v-bind="dates" v-model="dates.value"
                                    @callback="temporalChanged"></vue-slider>
                    </div>
                    <div class="aeris-eccad-datasearch-datasetClass">
                        <span class="aeris-eccad-datasearch-overviewTitle">{{$t('selecteddatasets')}}</span>
                        <ul class="aeris-eccad-datasearch-overviewSelectedDatasets">
                            <li :class="datasetClass(currentDataset)"
                                v-for="currentDataset in dataAdded"
                                v-on:click="datasetSelected(currentDataset)">
                                {{currentDataset.fullname_category}} - {{currentDataset.title_inventory}} -
                                {{currentDataset.fullname_resolution}} - {{currentDataset.fullname_geospatial}} <span
                                    v-if="currentDataset.display_name_scenario != 'None'">- {{currentDataset.display_name_scenario}}</span>
                                - {{currentDataset.display_name_parametre}}
                                <span v-on:click="removeToData(currentDataset)"
                                      class="aeris-eccad-datasearch-removeSpan">x</span>
                            </li>

                        </ul>
                    </div>
                </div>

                <div class="aeris-eccad-datasearch-bboxClass">
                    <div class="aeris-eccad-datasearch-boundingbox">
                        <span class="aeris-eccad-datasearch-overviewTitle">{{$t('boundingbox')}}</span>
                        <bbox @lonMaxR="updateLonMax" @lonMinR="updateLonMin" @latMaxR="updateLatMax"
                              @latMinR="updateLatMin"></bbox>
                    </div>
                    <div class="aeris-eccad-datasearch-select-add-reset-buttons">
                        <div type="button" class="btn btn-default aeris-eccad-datasearch-resetButton" name="select"
                             v-on:click="addToData()">{{$t('select')}}<span
                        ></span></div>
                        <div type="button" class="btn btn-default aeris-eccad-datasearch-resetButton" name="add"
                             v-on:click="reset()">{{$t('add')}}<span
                        ></span></div>
                        <div type="button" class="btn btn-default aeris-eccad-datasearch-resetButton" name="reset"
                             v-on:click="reset()">{{$t('reset')}}<span
                        ></span></div>
                    </div>

                    <div type="button" class="btn btn-default aeris-eccad-datasearch-gotoDataButton" name="accesdata"
                         v-on:click="gotoData()">{{$t('accessdata')}}<span
                    ></span></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import {EventBus} from '../aeris-event-bus.js';
    import bbox from './aeris-eccad-bbox';
    import vueSlider from 'vue-slider-component';
    import store from "../../../store/storeIndex";

    export default {
        name :"aeris-eccad-datasearch",
        components: {vueSlider, bbox},
        props: {},

        data() {
            return {
                alldata: [],
                allSectorInventogryGroup: [],
                allDates: [],
                selectedData: [],
                selectedDates: [],
                selectedAllSectorInventoryGroup: [],
                selectedCategorySize: '',
                selectedGroupOfSectorSize: '',
                selectedSectorSize: '',
                selectedParameterSize: '',
                selectedGroupOfSpecieSize: '',
                selectedGroupOfDatasetSize: '',
                selectedScenarioSize: '',
                selectedResolutionSize: '',
                selectedGeospatialSize: '',
                dataAdded: [],
                selectedDataset: '',
                dates: {
                    value: [],
                    width: '100%',
                    height: 6,
                    dotSize: 14,
                    min: 1,
                    max: 100,
                    interval: 3,
                    disabled: false,
                    show: true,
                    tooltip: 'always',
                    piecewise: true,
                    tooltipDir: [
                        "bottom",
                        "top"
                    ],
                    data: []
                },
                categoriesService: '/data/alldata',
            }
        },

        watch: {},

        mounted: function () {
            this.alldata = store.getters.allDatas;
            this.selectedData = this.alldata;

            var output = [];
            var keys = [];
            this.selectedData.forEach(function (data) {
                var key = data.start_date_inv_cat;
                if (keys.indexOf(key) === -1) {
                    keys.push(key);
                    output.push(String(data.start_date_inv_cat));
                }

                var key2 = data.end_date_inv_cat;
                if (keys.indexOf(key2) === -1) {
                    keys.push(key2);
                    output.push(String(data.end_date_inv_cat));
                }
            });
            this.allDates = output.sort();
            this.dates.value = [String(this.allDates[0]), String(this.allDates[this.allDates.length - 1])];
            this.dates.data = this.allDates;
            this.selectedDates = this.allDates;

            this.allSectorInventogryGroup = store.getters.allSectorInventogryGroup
            this.selectedAllSectorInventoryGroup = this.allSectorInventogryGroup;
        },

        updated: function () {
        },

        destroyed: function () {
        },

        created: function () {
        },

        computed: {

            uniqueCategories: function () {
                var currentComponent = this;
                return function (datatype) {
                    var output = [];
                    var keys = [];

                    currentComponent.selectedData.forEach(function (data) {
                        var key = data.fullname_category;

                        if (keys.indexOf(key) === -1) {
                            keys.push(key);
                            output.push(data);
                        }
                    });

                    function compare(category1, category2) {
                        if (category1.fullname_category < category2.fullname_category)
                            return -1;
                        if (category1.fullname_category > category2.fullname_category)
                            return 1;
                        return 0;
                    }

                    this.selectedCategorySize = output.length;
                    return output.sort(compare);
                };
            },
            uniqueParameters: function () {
                var currentComponent = this;
                return function (parameter) {
                    var output = [];
                    var keys = [];

                    currentComponent.selectedData.forEach(function (data) {
                        var key = data.display_name_parametre;

                        if (keys.indexOf(key) === -1) {
                            keys.push(key);
                            output.push(data);
                        }
                    });

                    function compare(parameter1, parameter2) {
                        if (parameter1.display_name_parametre < parameter2.display_name_parametre)
                            return -1;
                        if (parameter1.display_name_parametre > parameter2.display_name_parametre)
                            return 1;
                        return 0;
                    }

                    this.selectedParameterSize = output.length;
                    return output.sort(compare);
                };
            }
            ,
            uniqueParameterGroup: function () {
                var currentComponent = this;
                return function (paramGroup) {
                    var output = [];
                    var keys = [];

                    currentComponent.selectedData.forEach(function (data) {
                        var key = data.fullname_parametre_group;

                        if (keys.indexOf(key) === -1) {
                            keys.push(key);
                            output.push(data);
                        }
                    });

                    function compare(paramGroup1, paramGroup2) {
                        if (paramGroup1.fullname_parametre_group < paramGroup2.fullname_parametre_group)
                            return -1;
                        if (paramGroup1.fullname_parametre_group > paramGroup2.fullname_parametre_group)
                            return 1;
                        return 0;
                    }

                    this.selectedGroupOfSpecieSize = output.length;
                    return output.sort(compare);
                };
            }
            ,
            uniqueDatasetGroup: function () {
                var currentComponent = this;
                return function (datasetGroup) {
                    var output = [];
                    var keys = [];

                    currentComponent.selectedData.forEach(function (data) {
                        var key = data.title_inventory_group;

                        if (keys.indexOf(key) === -1) {
                            keys.push(key);
                            output.push(data);
                        }
                    });

                    function compare(datasetGroup1, datasetGroup2) {
                        if (datasetGroup1.title_inventory_group < datasetGroup2.title_inventory_group)
                            return -1;
                        if (datasetGroup1.title_inventory_group > datasetGroup2.title_inventory_group)
                            return 1;
                        return 0;
                    }

                    this.selectedGroupOfDatasetSize = output.length;
                    return output.sort(compare);
                };
            }
            ,
            uniqueResolution: function () {
                var currentComponent = this;
                return function (resolution) {
                    var output = [];
                    var keys = [];

                    currentComponent.selectedData.forEach(function (data) {
                        var key = data.fullname_resolution;

                        if (keys.indexOf(key) === -1) {
                            keys.push(key);
                            output.push(data);
                        }
                    });

                    function compare(resolution1, resolution2) {
                        if (resolution1.fullname_resolution < resolution2.fullname_resolution)
                            return -1;
                        if (resolution1.fullname_resolution > resolution2.fullname_resolution)
                            return 1;
                        return 0;
                    }

                    this.selectedResolutionSize = output.length;
                    return output.sort(compare);
                };
            }
            ,
            uniqueGeospatial: function () {
                var currentComponent = this;
                return function (geospatial) {
                    var output = [];
                    var keys = [];

                    currentComponent.selectedData.forEach(function (data) {
                        var key = data.fullname_geospatial;

                        if (keys.indexOf(key) === -1) {
                            keys.push(key);
                            output.push(data);
                        }
                    });

                    function compare(geospatial1, geospatial2) {
                        if (geospatial1.fullname_geospatial < geospatial2.fullname_geospatial)
                            return -1;
                        if (geospatial1.fullname_geospatial > geospatial2.fullname_geospatial)
                            return 1;
                        return 0;
                    }

                    this.selectedGeospatialSize = output.length;
                    return output.sort(compare);
                };
            },
            uniqueScenario: function () {
                var currentComponent = this;
                return function (scenario) {
                    var output = [];
                    var keys = [];

                    currentComponent.selectedData.forEach(function (data) {
                        var key = data.display_name_scenario;

                        if (keys.indexOf(key) === -1) {
                            keys.push(key);
                            output.push(data);
                        }
                    });

                    function compare(scenario1, scenario2) {
                        if (scenario1.order_scenario < scenario2.order_scenario)
                            return -1;
                        if (scenario1.order_scenario > scenario2.order_scenario)
                            return 1;
                        return 0;
                    }

                    this.selectedScenarioSize = output.length;
                    return output.sort(compare);
                };
            },
            uniqueSector: function () {
                var currentComponent = this;
                return function (sector) {
                    var output = [];
                    var keys = [];

                    currentComponent.selectedAllSectorInventoryGroup.forEach(function (sectorInventoryGroup) {
                        var key = sectorInventoryGroup.name_sector;

                        if (keys.indexOf(key) === -1) {
                            keys.push(key);
                            output.push(sectorInventoryGroup);
                        }
                    });

                    function compare(sector1, sector2) {
                        if (sector1.name_sector < sector2.name_sector)
                            return -1;
                        if (sector1.name_sector > sector2.name_sector)
                            return 1;
                        return 0;
                    }

                    this.selectedSectorSize = output.length;
                    return output.sort(compare);
                };
            },
            uniqueSectorGroup: function () {
                var currentComponent = this;
                return function (sectorGroup) {
                    var output = [];
                    var keys = [];

                    currentComponent.selectedAllSectorInventoryGroup.forEach(function (sectorInventoryGroup) {
                        var key = sectorInventoryGroup.name_sector_group;

                        if (keys.indexOf(key) === -1) {
                            keys.push(key);
                            output.push(sectorInventoryGroup);
                        }
                    });

                    function compare(sectorGroup1, sectorGroup2) {
                        if (sectorGroup1.name_sector_group < sectorGroup2.name_sector_group)
                            return -1;
                        if (sectorGroup1.name_sector_group > sectorGroup2.name_sector_group)
                            return 1;
                        return 0;
                    }

                    this.selectedGroupOfSectorSize = output.length;
                    return output.sort(compare);
                };
            }
        },

        methods: {

            reset: function () {
                this.resetAll();
            },

            resetAll: function () {
                this.selectedData = this.alldata;
                this.selectedAllSectorInventoryGroup = this.allSectorInventogryGroup;

                this.selectedDates = this.allDates;
                this.dates.data = this.selectedDates;
                this.dates.value = [String(this.selectedDates[0]), String(this.selectedDates[this.selectedDates.length - 1])];

                EventBus.$emit('resetGeoBox', "");
            },

            addToData: function () {
                var vm = this;
                this.selectedData.forEach(function (data) {
                    if (vm.dataAdded.indexOf(data) === -1) {
                        vm.dataAdded.push(data);
                    }
                });
            },

            removeToData: function (currentDataset) {
                var index = this.dataAdded.indexOf(currentDataset);
                if (index != null && index >= 0) {
                    this.dataAdded.splice(index, 1);
                }
            },

            gotoData: function () {
                store.commit('setSelectedDatas', this.dataAdded)
                EventBus.$emit('selectedCurrentDatasets', this.dataAdded)
                EventBus.$emit('dataSearchPanelClosed', '');
                this.dataAdded = [];
            },

            datatypeChanged: function (datatype) {
                var output = [];
                this.selectedData.forEach(function (data) {
                    var key = data.fullname_category;

                    if (datatype.fullname_category === key) {
                        output.push(data);
                    }
                });

                this.selectedData = output;
                this.updateAllSectorInventoryGroup();
            },

            speciesChanged: function (species) {
                var output = [];
                this.selectedData.forEach(function (data) {
                    var key = data.display_name_parametre;

                    if (species.display_name_parametre === key) {
                        output.push(data);
                    }
                });

                this.selectedData = output;
                this.updateAllSectorInventoryGroup();
            },

            inventoryGroupChanged: function (inventoryGroup) {
                var output = [];
                this.selectedData.forEach(function (data) {
                    var key = data.title_inventory_group;

                    if (inventoryGroup.title_inventory_group === key) {
                        output.push(data);
                    }
                });

                this.selectedData = output;
                this.updateAllSectorInventoryGroup();
            },

            sectorGroupChanged: function (sectorGroup) {
                var output = [];
                this.selectedAllSectorInventoryGroup.forEach(function (sectorInventoryGroup) {
                    var key = sectorInventoryGroup.name_sector_group;

                    if (sectorGroup.name_sector_group === key) {
                        output.push(sectorInventoryGroup);
                    }
                });

                this.selectedAllSectorInventoryGroup = output;
                this.updateSearchData(sectorGroup);
                this.updateAllSectorInventoryGroup();
            },

            sectorChanged: function (sector) {
                var output = [];
                this.selectedAllSectorInventoryGroup.forEach(function (sectorInventoryGroup) {
                    var key = sectorInventoryGroup.name_sector;

                    if (sector.name_sector === key) {
                        output.push(sectorInventoryGroup);
                    }
                });

                this.selectedAllSectorInventoryGroup = output;
                this.updateSearchData(sector);
                this.updateAllSectorInventoryGroup();
            },

            scenarioChanged: function (scenario) {
                var output = [];
                this.selectedData.forEach(function (data) {
                    var key = data.display_name_scenario;

                    if (scenario.display_name_scenario === key) {
                        output.push(data);
                    }
                });

                this.selectedData = output;
                this.updateAllSectorInventoryGroup();
            },

            speciesGroupChanged: function (speciesGroup) {
                var output = [];
                this.selectedData.forEach(function (data) {
                    var key = data.fullname_parametre_group;

                    if (speciesGroup.fullname_parametre_group === key) {
                        output.push(data);
                    }
                });

                this.selectedData = output;
                this.updateAllSectorInventoryGroup();
            },

            resolutionChanged: function (resolution) {
                var output = [];
                this.selectedData.forEach(function (data) {
                    var key = data.fullname_resolution;

                    if (resolution.fullname_resolution === key) {
                        output.push(data);
                    }
                });

                this.selectedData = output;
                this.updateAllSectorInventoryGroup();
            },

            geospatialChanged: function (geospatial) {
                var output = [];
                this.selectedData.forEach(function (data) {
                    var key = data.fullname_geospatial;

                    if (geospatial.fullname_geospatial === key) {
                        output.push(data);
                    }
                });
                this.selectedData = output;
                this.updateAllSectorInventoryGroup();

                var latmax = geospatial.latmax;
                var latmin = geospatial.latmin;
                var lonmax = geospatial.lonmax;
                var lonmin = geospatial.lonmin;
                EventBus.$emit('setGeoBox', {latmax, latmin, lonmax, lonmin});
            },

            temporalChanged: function (values) {
                var output = [];
                if (values[0] < this.dates.value[0] || values[1] > this.dates.value[1]) {
                    this.reset();
                }

                this.selectedData.forEach(function (data) {
                    if (values[0] <= data.start_date_inv_cat && data.start_date_inv_cat <= values[1]) {
                        output.push(data);
                    }
                    if (values[0] <= data.end_date_inv_cat && data.end_date_inv_cat <= values[1]) {
                        output.push(data);
                    }
                });
                this.selectedData = output;
                this.updateAllSectorInventoryGroup();
            },

            updateSectorInventoryGroup: function (inventory) {
                var output = [];
                this.selectedAllSectorInventoryGroup.forEach(function (sectorInventoryGroup) {
                    var key = sectorInventoryGroup.id_inventory;

                    if (key === inventory.id_inventory) {
                        output.push(sectorInventoryGroup);
                    }
                });

                if (output.length === 0) {
                    output.push(this.allSectorInventogryGroup[0]);
                }

                this.selectedAllSectorInventoryGroup = output;
                this.updateTemporal();
            },

            updateAllSectorInventoryGroup: function () {
                var currentComponent = this;
                var output = [];
                var keys = [];
                this.selectedData.forEach(function (data) {
                    currentComponent.selectedAllSectorInventoryGroup.forEach(function (sectorInventoryGroup) {

                        if (sectorInventoryGroup.id_inventory === data.id_inventory && keys.indexOf(sectorInventoryGroup) === -1) {
                            output.push(sectorInventoryGroup);
                            keys.push(sectorInventoryGroup);
                        }
                    });
                });

                if (output.length === 0) {
                    output.push(this.allSectorInventogryGroup[0]);
                }

                this.selectedAllSectorInventoryGroup = output;
            },

            updateSearchData: function (sectorInventoryGroup) {
                var output = [];
                var currentComponent = this;
                this.selectedData.forEach(function (data) {
                    var key = data.id_inventory;
                    var found = false;
                    currentComponent.allSectorInventogryGroup.forEach(function (sectorInventoryGroupTmp) {
                        if (sectorInventoryGroupTmp.id_inventory === key) {
                            found = true;
                        }
                    });

                    if (sectorInventoryGroup.id_inventory === -1 && !found && output.indexOf(data) === -1) {
                        output.push(data);
                    } else if (sectorInventoryGroup.id_inventory != -1 && found && output.indexOf(data) === -1) {
                        output.push(data);
                    }

                });

                this.selectedData = output;
                this.updateTemporal();
            },

            updateLonMax: function (lonMax) {
                var output = [];
                this.selectedData.forEach(function (data) {
                    if (lonMax >= data.lonmax) {
                        output.push(data);
                    }
                });

                this.selectedData = output;
                this.updateAllSectorInventoryGroup();
            }

            ,

            updateLonMin: function (lonMin) {
                var output = [];
                this.selectedData.forEach(function (data) {
                    if (data.lonmin >= lonMin) {
                        output.push(data);
                    }
                });

                this.selectedData = output;
                this.updateAllSectorInventoryGroup();
            }

            ,

            updateLatMax: function (latMax) {
                var output = [];
                this.selectedData.forEach(function (data) {
                    if (data.latmax <= latMax) {
                        output.push(data);
                    }
                });

                this.selectedData = output;
                this.updateAllSectorInventoryGroup();
            }

            ,

            updateLatMin: function (latMin) {
                var output = [];
                this.selectedData.forEach(function (data) {
                    if (data.latmin >= latMin) {
                        output.push(data);
                    }
                });

                this.selectedData = output;
                this.updateAllSectorInventoryGroup();
            },

            updateTemporal: function () {
                var output = [];
                var keys = [];
                this.selectedData.forEach(function (data) {
                    var key = data.start_date_inv_cat;
                    if (keys.indexOf(key) === -1) {
                        keys.push(key);
                        output.push(String(data.start_date_inv_cat));
                    }

                    var key2 = data.end_date_inv_cat;
                    if (keys.indexOf(key2) === -1) {
                        keys.push(key2);
                        output.push(String(data.end_date_inv_cat));
                    }
                });
                this.selectedDates = output.sort();
                this.dates.value = [String(this.selectedDates[0]), String(this.selectedDates[this.selectedDates.length - 1])];
                this.dates.data = this.selectedDates;

            },
            datatypeClass: function (datatype) {
                if (this.selectedCategorySize === 1) {
                    return 'aeris-eccad-datasearch-overviewSelected';
                } else {
                    return '';
                }
            },
            datasetGroupClass: function (dataset) {
                if (this.selectedGroupOfDatasetSize === 1) {
                    return 'aeris-eccad-datasearch-overviewSelected';
                } else {
                    return '';
                }
            },

            sectorClass: function (sector) {
                if (this.selectedSectorSize === 1 && this.selectedAllSectorInventoryGroup[0].id_sector != -1) {
                    return 'aeris-eccad-datasearch-overviewSelected';
                } else {
                    return '';
                }
            },

            sectorGroupClass: function (sector) {
                if (this.selectedGroupOfSectorSize === 1 && this.selectedAllSectorInventoryGroup[0].id_sector != -1) {
                    return 'aeris-eccad-datasearch-overviewSelected';
                } else {
                    return '';
                }
            },


            speciesClass: function (species) {
                if (this.selectedParameterSize === 1) {
                    return 'aeris-eccad-datasearch-overviewSelected';
                } else {
                    return '';
                }
            },

            speciesGroupClass: function (speciesGroup) {
                if (this.selectedGroupOfSpecieSize === 1 && this.selectedData[0].fullname_parametre_group != 'None') {
                    return 'aeris-eccad-datasearch-overviewSelected';
                } else {
                    return '';
                }
            },


            scenarioClass: function (scenario) {
                if (this.selectedScenarioSize === 1 && this.selectedData[0].id_scenario != -1) {
                    return 'aeris-eccad-datasearch-overviewSelected';
                } else {
                    return '';
                }

            },
            resolutionClass: function (res) {
                if (this.selectedResolutionSize === 1) {
                    return 'aeris-eccad-datasearch-overviewSelected';
                } else {
                    return '';
                }
            },

            geospatialClass: function (geospatial) {
                if (this.selectedGeospatialSize === 1) {
                    var latmax = geospatial.latmax;
                    var latmin = geospatial.latmin;
                    var lonmax = geospatial.lonmax;
                    var lonmin = geospatial.lonmin;
                    EventBus.$emit('setGeoBox', {latmax, latmin, lonmax, lonmin});
                    return 'aeris-eccad-datasearch-overviewSelected';
                } else {
                    return '';
                }
            },
            datasetClass: function (dataset) {
                if (this.selectedDataset != null && this.selectedDataset === dataset) {
                    return 'aeris-eccad-datasearch-overviewSelected aeris-eccad-datasearch-itemDatasets';
                } else {
                    return 'aeris-eccad-datasearch-itemDatasets';
                }
            },
            datasetSelected: function (dataset) {
                this.selectedDataset = dataset;
            }
        }
    }

</script>

<style>

    body {
        font-family: Arial Unicode MS, Arial, sans-serif;
        font-size: small;
        /*font-size: 14px; */
        line-height: 1.42857143;
        color: #333;
        background-color: #fff;
    }

    a {
        text-decoration: none;
        color: navy;
    }

    a:hover {
        text-decoration: none;
    }

    label {
        display: inline-block;
        max-width: 100%;
        margin-bottom: 5px;
        font-weight: 700;
    }

    /* OVERVIEW */

    .aeris-eccad-datasearch-overviewFlex {
        display: -webkit-flex;
        display: flex;
        -webkit-flex-direction: row;
        flex-direction: row;
        -webkit-justify-content: center;
        justify-content: center;
        padding: 10px;
    }

    .aeris-eccad-datasearch-overviewFlex1 {
        display: -webkit-flex;
        display: flex;
        -webkit-flex-direction: row;
        flex-direction: row;
        -webkit-justify-content: center;
        justify-content: center;
        padding: 10px;
        margin-top: -28px;
    }

    .aeris-eccad-datasearch-overviewList {
        list-style-type: none;
        padding: 3px;
        border: 1px solid gainsboro;
        border-radius: 3px;
        max-height: 188px;
        min-height: 188px;
        overflow: auto;
    }

    .aeris-eccad-datasearch-overviewListW {
        list-style-type: none;
        padding: 3px;
        border: 1px solid gainsboro;
        border-radius: 3px;
        max-height: 188px;
        min-height: 188px;
        overflow: auto;
    }

    .aeris-eccad-datasearch-overviewSelectedDatasets {
        display: flex;
        flex-direction: column;
        list-style-type: none;
        padding: 3px;
        border: 1px solid gainsboro;
        border-radius: 3px;
        max-height: 140px;
        min-height: 140px;
        overflow: auto;
    }

    .aeris-eccad-datasearch-overviewListM {
        list-style-type: none;
        padding: 3px;
        border: 1px solid gainsboro;
        border-radius: 3px;
        max-height: 188px;
        min-height: 188px;
        overflow: auto;
    }

    .aeris-eccad-datasearch-overviewListML {
        list-style-type: none;
        padding: 3px;
        border: 1px solid gainsboro;
        border-radius: 3px;
        max-height: 188px;
        min-height: 188px;
        overflow: auto;
        margin-bottom: 18px;
    }

    .aeris-eccad-datasearch-overviewListMLW {
        list-style-type: none;
        padding: 3px;
        border: 1px solid gainsboro;
        border-radius: 3px;
        max-height: 188px;
        min-height: 188px;
        overflow: auto;

    }

    .aeris-eccad-datasearch-overviewListL {
        list-style-type: none;
        padding: 3px;
        border: 1px solid gainsboro;
        border-radius: 3px;
        max-height: 188px;
        min-height: 188px;
        overflow: auto;
        margin-bottom: 18px;
    }

    .aeris-eccad-datasearch-overviewList > li:nth-child(2n+1), .aeris-eccad-datasearch-overviewListM > li:nth-child(2n+1), .aeris-eccad-datasearch-overviewListL > li:nth-child(2n+1), .aeris-eccad-datasearch-overviewListMLW > li:nth-child(2n+1), .aeris-eccad-datasearch-overviewListW > li:nth-child(2n+1), .aeris-eccad-datasearch-overviewListML > li:nth-child(2n+1) {
        color: navy;
        /* background-color: #FBFAF6; */
    }

    .aeris-eccad-datasearch-overviewList > li:nth-child(2n), .aeris-eccad-datasearch-overviewListM > li:nth-child(2n), .aeris-eccad-datasearch-overviewListL > li:nth-child(2n), .aeris-eccad-datasearch-overviewListMLW > li:nth-child(2n), .aeris-eccad-datasearch-overviewListW > li:nth-child(2n), .aeris-eccad-datasearch-overviewListML > li:nth-child(2n) {
        color: navy;
    }

    .aeris-eccad-datasearch-overviewTitle {
        margin-left: 13px;
        color: navy;
        font-weight: bold;
    }

    /* CUSTOM BADGE */
    .aeris-eccad-datasearch-eccadBadge {
        display: inline-block;
        margin-left: 2px;
        min-width: 10px;
        padding: 3px 7px;
        font-size: 12px;
        font-weight: 700;
        line-height: 1;
        color: black;
        text-align: center;
        white-space: nowrap;
        vertical-align: middle;
        background-color: #ecbf88;
        border-radius: 10px;
    }

    .aeris-eccad-datasearch-overviewSelected {
        background-color: #ecbf88;
    }

    .aeris-eccad-datasearch-overviewDisabled {
        pointer-events: none;
        /* display: none; */
        background-color: white;
    }

    .aeris-eccad-datasearch-sliderClass {
        margin-top: 20px;
    }

    .aeris-eccad-datasearch-resetButton {
        margin-top: 5px;
        min-width: 100px;
        background-color: #f49b1b;
        font-size: 120%;
        color: white;
        font-weight: bold;
        margin-top: 20px;
    }

    .aeris-eccad-datasearch-resetButton:hover {
        color: black;
    }

    .aeris-eccad-datasearch-gotoDataButton {
        min-width: 200px;
        background-color: #f49b1b;
        font-size: 120%;
        color: white;
        font-weight: bold;
        margin-top: 10px;
    }

    .aeris-eccad-datasearch-gotoDataButton:hover {
        color: black;
    }

    .aeris-eccad-datasearch-sectorClass {
        margin: 5px;
        flex: 1.5;
        -ms-flex: 1.58;
    }

    .aeris-eccad-datasearch-temporalClass {
        margin: 5px;
        flex: 2;
        -ms-flex: 2.15;
    }

    .aeris-eccad-datasearch-bboxClass {
        margin: 5px;
        flex: 1.05;
        -ms-flex: 1;
        margin-top: 14px;
    }

    .aeris-eccad-datasearch-removeSpan {
        background-color: gainsboro;
        min-width: 20px;
        min-height: 20px;
        float: right;
        margin: 1px;
    }

    .aeris-eccad-datasearch-overviewFlex,
    .aeris-eccad-datasearch-overviewFlex1 {
        flex-wrap: wrap;
    }

    .aeris-eccad-datasearch-overviewFlex > * {
        padding: 5px;
        flex: 0 1 250px;
    }

    .aeris-eccad-datasearch-overviewFlex > :nth-child(1) {
        flex: 0 1 300px;
    }

    .aeris-eccad-datasearch-overviewFlex > :nth-child(5) {
        flex: 0 1 300px;
    }

    .aeris-eccad-datasearch-overviewFlex > :nth-child(9) {
        flex: 0 1 300px;
    }

    .aeris-eccad-datasearch-overviewFlex > :nth-child(10) {
        flex: 0 1 500px;
    }

    .aeris-eccad-datasearch-overviewFlex1 > :nth-child(11) {
        flex: 0 1 200px;
    }

</style>


