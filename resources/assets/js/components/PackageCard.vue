<template>
    <div class="flex m-2 mb-4 shadow hover:shadow-md h-128 w-full max-w-xs">
        <div
        :style="{'border': '1px solid #ddd', 'border-top-width': '4px', 'border-top-color': package.accent}"
        class="flex-1 bg-white text-sm rounded-sm"
        >
            <div
            v-if="novapackages.is_admin == true"
            class="text-right -mb-6"
            >
                <admin-dropdown>
                    <span slot="link" class="appearance-none flex items-center inline-block text-white font-medium">
                        <svg class="h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                            <path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z" />
                        </svg>
                    </span>

                    <div slot="dropdown" class="bg-indigo shadow rounded border overflow-hidden">
                        <a
                            v-if="package.is_disabled == true"
                            :href="route('app.admin.enable-package', package)"
                            class="no-underline block px-4 py-3 border-b text-white bg-indigo hover:text-white hover:bg-blue"
                        >
                            Enable
                        </a>

                        <a
                            v-else
                            :href="route('app.admin.disable-package', package)"
                            class="no-underline block px-4 py-3 border-b text-white bg-indigo hover:text-white hover:bg-blue"
                        >
                            Disable
                        </a>
                    </div>
                </admin-dropdown>
            </div>

            <div class="flex flex-row mt-4 px-4 pb-4"  style="height: 14em">
                <div class="pb-2 w-full relative">
                    <a
                        :href="route('packages.show', { 'namespace': package.packagist_namespace, 'name': package.packagist_name})"
                        class="block mb-2 no-underline"
                    >
                        <h2 class="text-xl font-bold text-gray-100 flex flex-row items-center">
                            <title-icon
                                :color="package.accent"
                                size="small"
                                :title="package.name"
                            ></title-icon>

                            {{ novaLessName }}
                            <span class="text-xs uppercase text-gray-400" v-if="package.is_disabled == true">Disabled</span>
                        </h2>
                    </a>

                    <div class="flex flex-row absolute bottom-0 right-0">
                        <div class="flex">
                            <star-rating
                                v-bind:rating="floatAverageRating"
                                :increment="0.1"
                                v-bind:read-only="true"
                                v-bind:star-size="20"
                                v-bind:show-rating="false"></star-rating>
                        </div>

                        <div class="flex text-gray-500 pt-1 pl-1 text-xs">
                            ({{ package.rating_count }})
                        </div>
                    </div>

                    <div
                        v-html="package.abstract"
                        class="text-gray-800 leading-normal mb-4 markdown leading-tight w-full" style="word-break: break-word;"
                    ></div>

                    <a
                        :href="route('packages.show', { 'namespace': package.packagist_namespace, 'name': package.packagist_name})"
                        class="absolute block text-indigo-600 font-bold no-underline bottom-0 left-0"
                    >
                        Learn More
                    </a>
                </div>
            </div>

            <div class="bg-gray-100 flex text-sm border-t border-gray-300 px-4 py-4 items-center">
                <img :src="package.author.avatar_url" class="rounded-full h-6 w-6 mr-4" :alt="package.author.name" />

                <a
                    :href="'/collaborators/' + package.author.github_username"
                    class="text-indigo-600 font-bold no-underline uppercase text-xs hover:text-indigo-700"
                >
                    {{ package.author.name }}
                </a>
            </div>
        </div>
    </div>
</template>

<script>
import _ from 'lodash';
import StarRating from 'vue-star-rating';

export default {
    components: { StarRating },
    props: ['package'],
    methods: {
        startCase: function(string) {
            return _.startCase(string);
        },
    },
    computed: {
        novaLessName: function() {
            return this.package.name
                .replace(/^(Nova\ )/,"")
                .replace(/^(Laravel\ Nova\ )/,"")
                .replace(/(\ for\ Nova)/,"");
        },
        floatAverageRating: function() {
            return parseFloat(this.package.average_rating);
        },
    },
};
</script>
