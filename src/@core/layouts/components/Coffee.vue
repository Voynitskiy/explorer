<template>
  <div>
    <b-card title="Thanks for donation">
      Your donation is really important for us to build better open source product.
      <div class="mt-1">
        ERC20 Address:
      </div>
      <b-input-group>
        <b-input-group-prepend>
          <b-avatar
            src="/logos/ethereum.png"
            class="mr-50"
          />
        </b-input-group-prepend>
        <b-form-input :value="erc20" />
      </b-input-group>
    </b-card>
    <b-card>
      <b-row>
        <b-col />
      </b-row>
      <b-row><b-col>
        <div class="mt-1">
          Or any tokens you like:
        </div>
        <hr></b-col>
      </b-row>
      <b-row>
        <b-col
          v-for="item in chains"
          :key="item.chain"
          md="3"
          lg="2"
        >
          <b-button
            v-ripple.400="'rgba(113, 102, 240, 0.15)'"
            variant="outline-primary"
            :to="`/${item.chain}/account/${item.addr}/receive`"
            block
            class="text-left text-truncate mt-1"
          >
            <b-avatar
              :src="item.icon"
              size="sm"
              variant="transparent"
            />
            <span class="ml-1 text-uppercase text-truncate">{{ item.chain }}</span>
          </b-button>
        </b-col>
      </b-row>
    </b-card>
  </div>
</template>

<script>
import {
  BRow, BCol, BLink, BButton, BCard,
  BAvatar, BFormInput, BInputGroupPrepend, BInputGroup,
} from 'bootstrap-vue'
import Ripple from 'vue-ripple-directive'
import {
  addressEnCode, addressDecode,
} from '@/libs/utils'

export default {
  name: 'Coffee',
  components: {
    BButton,
    BCol,
    BCard,
    BRow,
    BLink,
    BAvatar,
    BFormInput,
    BInputGroupPrepend,
    BInputGroup,
  },
  directives: {
    Ripple,
  },
  data() {
    return {
      erc20: '0x1f70a6D6F92F61ead83c5a08D3DAb3523EBdf82d',
      selectedAddress: 'cosmos1qsagnxelevekw4vlqe6n62mjaw0rggw0m32tj4',
    }
  },
  computed: {
    chains() {
      const { data } = addressDecode('cosmos1qsagnxelevekw4vlqe6n62mjaw0rggw0m32tj4')
      const config = Object.values(JSON.parse(localStorage.getItem('chains')))
        .map(x => ({
          chain: x.chain_name,
          addr: addressEnCode(x.addr_prefix, data),
          icon: x.logo,
        }))
      return config
    },
  },
  methods: {
    transfer(addr) {
      this.selectedAddress = addr
      this.$bvModal.show('transfer-window')
    },
  },
}
</script>
