<script>
    export default {
        computed: {
            isDesc() {
                return this.$parent.query['sort[order]'] === 'desc';
            },
            icon() {
                return this.isDesc ? 'keyboard-arrow-down' : 'keyboard-arrow-up';
            }
        },

        methods: {
            toggle() {
                this.$parent.query['sort[order]'] = this.isDesc ? 'asc' : 'desc';
            }
        }
    }
</script>

<template>
    <div class="row">
        <div class="col-12 d-flex align-items-center">
            <div class="form-group mr-3">
                <label for="media-search" class="sr-only">{{ __('Search') }}</label>
                <input
                    id="media-search"
                    type="text"
                    class="form-control form-control-sm"
                    v-model.lazy="$parent.query.search"
                    v-debounce="300"
                    :placeholder="__('Search...')"
                    :disabled="$parent.busy"
                >
            </div>
            <div class="form-group mr-3">
                <div class="input-group input-group-sm">
                    <label for="media-type" class="input-group-prepend mb-0">
                        <span class="input-group-text">
                            <span>{{ __('Type') }}</span>
                        </span>
                    </label>
                    <select
                        id="media-type"
                        class="custom-select form-control"
                        v-model="$parent.query.type"
                        :disabled="$parent.busy"
                    >
                        <option value="all">{{ __('All') }}</option>
                        <option value="file">{{ __('Files') }}</option>
                        <option value="image">{{ __('Images') }}</option>
                    </select>
                </div>
            </div>
            <div class="form-group">
                <div class="input-group input-group-sm">
                    <label for="media-date" class="input-group-prepend mb-0">
                        <span class="input-group-text">
                            <span>{{ __('Sort') }}</span>
                        </span>
                    </label>
                    <select
                        id="media-date"
                        class="custom-select form-control"
                        v-model="$parent.query['sort[by]']"
                        :disabled="$parent.busy"
                    >
                        <option value="name">{{ __('Name') }}</option>
                        <option value="created_at">{{ __('Created at') }}</option>
                        <option value="updated_at">{{ __('Updated at') }}</option>
                    </select>
                    <div class="input-group-append">
                        <button class="btn btn-primary p-0" type="button" @click.prevent="toggle">
                            <icon :name="icon"></icon>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
