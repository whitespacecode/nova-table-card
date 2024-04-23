<template>
    <div class="overflow-x-auto bg-white dark:bg-gray-800 rounded-lg shadow h-full">
        <div v-if="title || useViewAll" 
            class="flex items-center justify-between py-3 text-sm px-6">
            <h1 v-if="title" class="font-bold">
                {{ title }}
            </h1>
            <div v-if="useViewAll && !viewAllBottomPosition">
                <a :class="viewAllUseButton ?
                        'flex-shrink-0 shadow rounded focus:outline-none ring-primary-200 dark:ring-gray-600 focus:ring bg-primary-500 hover:bg-primary-400 active:bg-primary-600 text-white dark:text-gray-800 inline-flex items-center font-bold px-3 h-8 text-xs' :
                        'link-default'" 
                    :href="viewAll.link"
                >
                    {{ viewAll.label ?? 'View All' }}
                </a>
            </div>
        </div>
        <table
            class="w-full"
            data-testid="resource-table"
            ref="table"
        >
            <TableHeader
                :fields="header"
                :should-show-column-borders="shouldShowColumnBorders"
                :has-view-column="hasViewColumn"
            />
            <tbody>
                <TableRow v-for="(row, index) in rows"
                          :key="index"
                          :row="row"
                          :should-show-tight="shouldShowTight"
                          :should-show-column-borders="shouldShowColumnBorders"
                          :has-view-column="hasViewColumn"
                />
            </tbody>
        </table>
        <div v-if="useViewAll &&viewAllBottomPosition" 
            class="w-full border-t border-gray-200 dark:border-gray-700 rounded-b-lg flex justify-center py-3"
        >
            <a :class="viewAllUseButton ?
                        'flex-shrink-0 shadow rounded focus:outline-none ring-primary-200 dark:ring-gray-600 focus:ring bg-primary-500 hover:bg-primary-400 active:bg-primary-600 text-white dark:text-gray-800 inline-flex items-center font-bold px-3 h-8 text-xs' :
                        'link-default'" 
                :href="viewAll.link"
                >
                {{ viewAll.label ?? 'View All' }}
            </a>
        </div>

        <Pagination v-if="paginator" v-model="paginator" @update-rows="update"></Pagination>
    </div>
</template>

<script>
import TableHeader from './TableHeader.vue';
import TableRow from './TableRow.vue';
import Pagination from './Pagination.vue';

export default {
    props: ['card'],

    components: {
        TableRow,
        TableHeader,
        Pagination
    },

    data() {
        return {
            rows: [],
            header: [],
            title: '',
            viewAll: null,
            paginator: null,
        }
    },
    computed: {
        hasViewColumn() {
            return this.rows.find((row) => row.view);
        },

        shouldShowTight() {
            return this.card.style === 'tight';
        },

        /**
         * Determine if the resource table should show column borders.
         */
        shouldShowColumnBorders() {
            return !! this.card.showBorders;
        },
        useViewAll() {
            return this.viewAll && this.viewAll.link;
        },
        viewAllUseButton() {
            return this.viewAll && this.viewAll.style && this.viewAll.style === 'button';
        },
        viewAllBottomPosition() {
            return this.viewAll && this.viewAll.position && this.viewAll.position === 'bottom';
        }
    },
    methods: {
        update(event){
            this.rows = event;
        }
    },
    created () {
		const {header, rows, title, paginator, viewAll} = this.card;

		this.header = header;
		this.title = title;
        this.rows = rows;
        this.paginator = paginator;
        this.viewAll = viewAll;
	},
}
</script>
