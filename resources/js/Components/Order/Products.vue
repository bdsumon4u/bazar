<script>
    export default {
        inject: ['form'],

        watch: {
            'form.fields.products'(n) {
                n.filter(product => ! product.item).forEach(product => {
                    this.$set(product, 'item', {
                        tax: 0,
                        quantity: 1,
                        price: this.price(product)
                    });
                });
            }
        },

        methods: {
            price(product) {
                return product.prices[this.form.fields.currency].default || 0;
            },
            total(product) {
                let total = (Number(product.item.price) + Number(product.item.tax || 0))
                    * Number(product.item.quantity || 1);

                return Number(total).toFixed(2);
            },
            remove(index) {
                this.form.fields.products.splice(index, 1);
            }
        }
    }
</script>

<template>
    <div class="table-responsive">
        <table v-show="form.fields.products.length" class="table table-hover has-filled-header mb-0 mt-3">
            <thead>
                <tr>
                    <th scope="col">{{ __('Name') }}</th>
                    <th scope="col">
                        {{ __('Price') }}
                        <span class="badge badge-light text-uppercase">{{ form.fields.currency }}</span>
                    </th>
                    <th scope="col">
                        {{ __('Tax') }}
                        <span class="badge badge-light text-uppercase">{{ form.fields.currency }}</span>
                    </th>
                    <th scope="col">{{ __('Qty') }}</th>
                    <th scope="col">{{ __('Total') }}
                        <span class="badge badge-light text-uppercase">{{ form.fields.currency }}</span>
                    </th>
                    <th scope="col"></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(product, index) in form.fields.products" :key="index">
                    <td>{{ product.name }}</td>
                    <td>
                        <form-input
                            class="mb-0 form-group-sm"
                            type="number"
                            placeholder="0"
                            step="0.01"
                            min="0"
                            size="3"
                            v-model="product.item.price"
                        ></form-input>
                    </td>
                    <td>
                        <form-input
                            class="mb-0 form-group-sm"
                            type="number"
                            placeholder="0"
                            step="0.01"
                            min="0"
                            size="3"
                            v-model="product.item.tax"
                        ></form-input>
                    </td>
                    <td>
                        <form-input
                            class="mb-0 form-group-sm"
                            type="number"
                            placeholder="1"
                            step="0.01"
                            min="0"
                            size="3"
                            v-model="product.item.quantity"
                        ></form-input>
                    </td>
                    <td>{{ total(product) }}</td>
                    <td>
                        <button
                            type="button"
                            class="icon-btn icon-btn-danger"
                            :aria-label="__('Remove')"
                            @click.prevent="remove(index)"
                        >
                            <icon name="close"></icon>
                        </button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
