<script setup>
import { computed, ref } from 'vue'
import {
  Info,
  TriangleAlert,
} from '@lucide/vue'
import Checkbox from "../../Checkbox.vue";

const props = defineProps([
  'platform',
  'config'
])

const defaultMoonlightPort = 47989

const config = ref(props.config)
const effectivePort = computed(() => +config.value?.port ?? defaultMoonlightPort)

const pcNumber = computed({
  get() {
    return Math.max(1, Math.floor((effectivePort.value - 47989) / 1000) + 1);
  },
  set(newValue) {
    const val = Math.max(1, Math.floor(newValue));
    if (config.value) {
      config.value.port = 47989 + (val - 1) * 1000;
      config.value.port_starting = 0;
      config.value.port_ending = 0;
    }
  }
})
</script>

<template>
  <div id="network" class="config-page">
    <!-- UPnP -->
    <Checkbox class="mb-3"
              id="upnp"
              locale-prefix="config"
              v-model="config.upnp"
              default="false"
    ></Checkbox>

    <!-- Address family -->
    <div class="mb-3">
      <label for="address_family" class="form-label">{{ $t('config.address_family') }}</label>
      <select id="address_family" class="form-select" v-model="config.address_family">
        <option value="ipv4">{{ $t('config.address_family_ipv4') }}</option>
        <option value="both">{{ $t('config.address_family_both') }}</option>
      </select>
      <div class="form-text">{{ $t('config.address_family_desc') }}</div>
    </div>

    <!-- Bind address -->
    <div class="mb-3">
      <label for="bind_address" class="form-label">{{ $t('config.bind_address') }}</label>
      <input type="text" class="form-control" id="bind_address" v-model="config.bind_address" />
      <div class="form-text">{{ $t('config.bind_address_desc') }}</div>
    </div>

    <!-- PC Number -->
    <div class="mb-3">
      <label for="pc_number" class="form-label">PC Number</label>
      <input type="number" min="1" max="99" class="form-control" id="pc_number" v-model="pcNumber" />
      <div class="form-text">Enter the unique number of this PC (e.g., 1, 2, 3). This automatically configures all ports.</div>
    </div>
    
    <div class="alert alert-success mt-3 mb-4">
      <h4 class="alert-heading">Router Port Forwarding Instructions</h4>
      <p>Please log into your TP-Link router and forward the following port range to this PC's local IP address:</p>
      <hr>
      <p class="mb-0">
        <strong>Forward Ports:</strong> {{ effectivePort - 5 }} - {{ effectivePort + 21 }} <br/>
        <strong>Protocol:</strong> All (TCP & UDP)
      </p>
    </div>
    <div class="alert alert-warning" v-if="config.origin_web_ui_allowed === 'wan'">
      <TriangleAlert :size="20" /> {{ $t('config.port_warning') }}
    </div>

    <!-- Origin Web UI Allowed -->
    <div class="mb-3">
      <label for="origin_web_ui_allowed" class="form-label">{{ $t('config.origin_web_ui_allowed') }}</label>
      <select id="origin_web_ui_allowed" class="form-select" v-model="config.origin_web_ui_allowed">
        <option value="pc">{{ $t('config.origin_web_ui_allowed_pc') }}</option>
        <option value="lan">{{ $t('config.origin_web_ui_allowed_lan') }}</option>
        <option value="wan">{{ $t('config.origin_web_ui_allowed_wan') }}</option>
      </select>
      <div class="form-text">{{ $t('config.origin_web_ui_allowed_desc') }}</div>
    </div>

    <!-- CSRF Allowed Origins -->
    <div class="mb-3">
      <label for="csrf_allowed_origins" class="form-label">{{ $t('config.csrf_allowed_origins') }}</label>
      <input type="text"
             class="form-control"
             id="csrf_allowed_origins"
             v-model="config.csrf_allowed_origins" />
      <div class="form-text">{{ $t('config.csrf_allowed_origins_desc') }}</div>
    </div>

    <!-- External IP -->
    <div class="mb-3">
      <label for="external_ip" class="form-label">{{ $t('config.external_ip') }}</label>
      <input type="text" class="form-control" id="external_ip" placeholder="123.456.789.12" v-model="config.external_ip" />
      <div class="form-text">{{ $t('config.external_ip_desc') }}</div>
    </div>

    <!-- LAN Encryption Mode -->
    <div class="mb-3">
      <label for="lan_encryption_mode" class="form-label">{{ $t('config.lan_encryption_mode') }}</label>
      <select id="lan_encryption_mode" class="form-select" v-model="config.lan_encryption_mode">
        <option value="0">{{ $t('_common.disabled_def') }}</option>
        <option value="1">{{ $t('config.lan_encryption_mode_1') }}</option>
        <option value="2">{{ $t('config.lan_encryption_mode_2') }}</option>
      </select>
      <div class="form-text">{{ $t('config.lan_encryption_mode_desc') }}</div>
    </div>

    <!-- WAN Encryption Mode -->
    <div class="mb-3">
      <label for="wan_encryption_mode" class="form-label">{{ $t('config.wan_encryption_mode') }}</label>
      <select id="wan_encryption_mode" class="form-select" v-model="config.wan_encryption_mode">
        <option value="0">{{ $t('_common.disabled') }}</option>
        <option value="1">{{ $t('config.wan_encryption_mode_1') }}</option>
        <option value="2">{{ $t('config.wan_encryption_mode_2') }}</option>
      </select>
      <div class="form-text">{{ $t('config.wan_encryption_mode_desc') }}</div>
    </div>

    <!-- Ping Timeout -->
    <div class="mb-3">
      <label for="ping_timeout" class="form-label">{{ $t('config.ping_timeout') }}</label>
      <input type="text" class="form-control" id="ping_timeout" placeholder="10000" v-model="config.ping_timeout" />
      <div class="form-text">{{ $t('config.ping_timeout_desc') }}</div>
    </div>

    <!-- Packet Size Limit -->
    <div class="mb-3">
      <label for="packetsize" class="form-label">{{ $t('config.packetsize') }}</label>
      <input type="number" min="0" max="65535" class="form-control" id="packetsize" placeholder="0" v-model="config.packetsize" />
      <div class="form-text">{{ $t('config.packetsize_desc') }}</div>
    </div>

  </div>
</template>

<style scoped>

</style>
