<script>
    export default {
        data() {
            return {
                title: this.__('Orders')
            };
        },

        methods: {
            badgeClass(status) {
                switch (status) {
                    case 'completed':
                        return 'badge-success';
                    case 'failed':
                    case 'cancelled':
                        return 'badge-danger';
                    case 'on_hold':
                        return 'badge-light';
                    default:
                        return 'badge-warning';
                }
            },
            formatDate(date) {
                return date.substr(0, 16).replace('T', ' ');
            }
        },

        computed: {
            url() {
                return window.location.href.replace(window.location.search, '').replace(/\/$/, '');
            }
        }
    }
</script>

<template>
    <card :title="title">
        <template #header>
            <inertia-link :href="`${url}/create`" class="btn btn-primary btn-sm">
                {{ __('Create Order') }}
            </inertia-link>
        </template>
        <data-table :response="$page.results" :filters="$page.filters" searchable>
            <data-column :label="__('ID')" sort="id">
                <template #default="item">
                    <inertia-link :href="`${url}/${item.id}`">
                        #{{ item.id }}
                    </inertia-link>
                </template>
            </data-column>
            <data-column :label="__('Total')">
                <template #default="item">
                    {{ item.formatted_total }}
                </template>
            </data-column>
            <data-column :label="__('Customer')">
                <template #default="item">
                    {{ item.address.name }}
                </template>
            </data-column>
            <data-column :label="__('Status')" sort="status">
                <template #default="item">
                    <span class="badge" :class="badgeClass(item.status)">
                        {{ item.status_name }}
                    </span>
                </template>
            </data-column>
            <data-column :label="__('Created at')" sort="created_at">
                <template #default="item">
                    {{ formatDate(item.created_at) }}
                </template>
            </data-column>
        </data-table>
    </card>
</template>
