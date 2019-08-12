<template>
    <div class="card text-center m-3">
        <h3 class="card-header">Vue.js + Node - Server Side Pagination Example</h3>
        <div class="card-body">
            <div v-for="item in pageOfItems" :key="item.id">{{item.name}}</div>
        </div>
        <div class="card-footer pb-0 pt-3">
            <ul v-if="pager.pages && pager.pages.length" class="pagination">
                <li :class="{'disabled':pager.currentPage === 1}" class="page-item first-item">
                    <router-link :to="{ query: { page: 1 }}" class="page-link">First</router-link>
                </li>
                <li :class="{'disabled':pager.currentPage === 1}" class="page-item previous-item">
                    <router-link :to="{ query: { page: pager.currentPage - 1 }}" class="page-link">Previous</router-link>
                </li>
                <li v-for="page in pager.pages" :key="page" :class="{'active':pager.currentPage === page}" class="page-item number-item">
                    <router-link :to="{ query: { page: page }}" class="page-link">{{page}}</router-link>
                </li>
                <li :class="{'disabled':pager.currentPage === pager.totalPages}" class="page-item next-item">
                    <router-link :to="{ query: { page: pager.currentPage + 1 }}" class="page-link">Next</router-link>
                </li>
                <li :class="{'disabled':pager.currentPage === pager.totalPages}" class="page-item last-item">
                    <router-link :to="{ query: { page: pager.totalPages }}" class="page-link">Last</router-link>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            pager: {},
            pageOfItems: []
        }
    },
    watch: {
        '$route.query.page': {
            immediate: true,
            handler(page) {
                page = parseInt(page) || 1;
                if (page !== this.pager.currentPage) {
                    fetch(`/api/items?page=${page}`, { method: 'GET' })
                        .then(response => response.json())
                        .then(({pager, pageOfItems}) => {
                            this.pager = pager;
                            this.pageOfItems = pageOfItems;
                        });
                }
            }
        }
    }
}
</script>
