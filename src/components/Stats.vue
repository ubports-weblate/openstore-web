Sta<template>
    <div class="row">
        <h1>Stats</h1>

        <h2 v-if="error" class="center text-red">
            There was an error trying to load the stats, please refresh and try again.
        </h2>

        <div v-if="loading" class="center">
            <i class="fa fa-spinner fa-spin fa-2x"></i>
        </div>

        <div v-if="!loading">
            <h2>App Types</h2>
            <table>
                <thead>
                    <tr>
                        <th>App Type</th>
                        <th>Count</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(count, name) in types" v-if="name != 'push'">
                        <td>
                            {{humanType(name)}}
                        </td>
                        <td>
                            {{count}}
                        </td>
                    </tr>
                </tbody>
            </table>

            <h2>Categories</h2>
            <table>
                <thead>
                    <tr>
                        <th>Category</th>
                        <th>Count</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(count, name) in categories">
                        <td>
                            {{name || 'Uncategorized'}}
                        </td>
                        <td>
                            {{count}}
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
import api from '@/api';
import opengraph from '@/opengraph';

export default {
    name: 'Stats',
    head: {
        title: {inner: 'Stats'},
        meta: function() {
            return opengraph({
                title: 'Stats - OpenStore',
            });
        },
    },
    data() {
        return {
            categories: [],
            types: [],
            loading: false,
            error: false,
        };
    },
    created() {
        this.loading = true;
        api.apps.stats().then((stats) => {
            this.loading = false;

            this.categories = stats.categories;
            this.types = stats.types;
        }).catch(() => {
            this.loading = false;
            this.error = true;
        });
    },
    methods: {
        humanType(type) {
            return type.replace('webapp', 'web app').replace('snappy', 'snap').replace(/\w\S*/g, (txt) => {
                return txt.charAt(0).toUpperCase() + txt.substr(1).toLowerCase(); // To title Case
            });
        },
    },
};
</script>
