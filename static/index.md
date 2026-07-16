# About

Maintained by Shenzhen Athom Technology Co., Ltd., China, this ESPHome project provides configuration files and pre-built firmware for supported Athom devices.

# Installation

Choose a firmware version and device, then connect it over USB to install the matching pre-built firmware.

<section id="installer" class="installer" aria-label="Firmware installer" aria-disabled="true">
  <div class="controls">
    <label class="field" for="version-select">
      <span>Firmware version</span>
      <select id="version-select"></select>
    </label>
    <label class="field" for="device-select">
      <span>Device</span>
      <select id="device-select"></select>
    </label>
    <label class="field" for="device-search">
      <span>Search</span>
      <input id="device-search" type="search" placeholder="Filter devices" autocomplete="off">
    </label>
  </div>

  <div id="device-grid" class="device-grid" aria-label="Available devices"></div>

  <div class="install-box">
    <div>
      <h3 id="selected-title">Select firmware</h3>
      <p id="selected-detail">The installer will use the selected version and device manifest.</p>
    </div>
    <esp-web-install-button id="install-button" manifest="firmware/Athom-ESP8285-Device.manifest.json" disabled></esp-web-install-button>
  </div>

  <p id="status" class="status" role="status"></p>
</section>

<style>
  .installer {
    margin-top: 1.5rem;
  }

  .controls {
    display: grid;
    grid-template-columns: minmax(150px, 0.7fr) minmax(210px, 1fr) minmax(170px, 0.8fr);
    gap: 14px;
    margin-bottom: 18px;
  }

  .field {
    display: grid;
    gap: 6px;
    color: #333;
    font-weight: 700;
  }

  .field select,
  .field input {
    box-sizing: border-box;
    width: 100%;
    min-height: 42px;
    border: 1px solid #c6cbd1;
    border-radius: 6px;
    padding: 8px 10px;
    background: #fff;
    color: #222;
    font: inherit;
  }

  .field select:disabled,
  .field input:disabled {
    background: #f4f6f8;
    color: #68727d;
    cursor: not-allowed;
  }

  .device-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 10px;
    margin-bottom: 20px;
  }

  .device-card {
    min-height: 60px;
    border: 1px solid #d3d7dc;
    border-radius: 6px;
    padding: 10px 12px;
    background: #fff;
    color: #24292f;
    text-align: left;
    cursor: pointer;
    font: inherit;
  }

  .device-card:hover,
  .device-card:focus {
    border-color: #1e9fea;
    outline: none;
  }

  .device-card[aria-pressed="true"] {
    border-color: #1e9fea;
    box-shadow: inset 4px 0 0 #1e9fea;
  }

  .device-name {
    display: block;
    font-weight: 700;
    line-height: 1.25;
  }

  .device-meta {
    display: block;
    margin-top: 4px;
    color: #65717c;
    font-size: 0.82rem;
    line-height: 1.2;
    overflow-wrap: anywhere;
  }

  .empty {
    grid-column: 1 / -1;
    margin: 0;
    color: #65717c;
  }

  .install-box {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 20px;
    border-top: 1px solid #d8dde3;
    padding-top: 18px;
  }

  .install-box h3 {
    margin: 0 0 4px;
    color: #24292f;
  }

  .install-box p,
  .status {
    margin: 0;
    color: #4d5660;
  }

  .status {
    margin-top: 12px;
    min-height: 1.4em;
  }

  .installer[aria-disabled="true"] esp-web-install-button {
    opacity: 0.55;
    pointer-events: none;
  }

  @media (max-width: 760px) {
    .controls {
      grid-template-columns: 1fr;
    }

    .install-box {
      align-items: flex-start;
      flex-direction: column;
    }
  }
</style>

<script type="module" src="https://unpkg.com/esp-web-tools@10/dist/web/install-button.js?module"></script>
<script>
  const state = {
    versions: [],
    selectedVersion: "",
    selectedDevice: "",
    filter: "",
    ready: false
  };

  const installer = document.querySelector("#installer");
  const versionSelect = document.querySelector("#version-select");
  const deviceSelect = document.querySelector("#device-select");
  const search = document.querySelector("#device-search");
  const grid = document.querySelector("#device-grid");
  const installButton = document.querySelector("#install-button");
  const title = document.querySelector("#selected-title");
  const detail = document.querySelector("#selected-detail");
  const status = document.querySelector("#status");

  function formatDeviceName(id) {
    return id
      .split("-")
      .map((part) => part.charAt(0).toUpperCase() + part.slice(1))
      .join(" ");
  }

  function normalizeDevice(device, version) {
    return {
      id: device.id,
      name: device.name || formatDeviceName(device.id),
      yaml: device.yaml || `${device.id}.yaml`,
      manifest: device.manifest || `firmware/${version}/${device.id}.manifest.json`
    };
  }

  function normalizeVersion(version) {
    return {
      version: version.version,
      tag: version.tag || `esphome-${version.version}`,
      devices: (version.devices || []).map((device) => normalizeDevice(device, version.version))
    };
  }

  async function loadInstallerIndex() {
    try {
      const response = await fetch("firmware/versions.json", { cache: "no-store" });
      if (!response.ok) {
        throw new Error(`versions.json returned ${response.status}`);
      }

      const data = await response.json();
      state.versions = (data.versions || []).map(normalizeVersion);
      if (state.versions.length === 0) {
        throw new Error("versions.json does not include firmware versions");
      }

      const latestVersion = state.versions.find((item) => item.version === data.latest);
      state.selectedVersion = latestVersion?.version || state.versions[0]?.version || "";
      state.ready = true;
      status.textContent = `Loaded ${state.versions.length} firmware version${state.versions.length === 1 ? "" : "s"}; latest is ${state.selectedVersion}.`;
    } catch (error) {
      state.versions = [];
      state.selectedVersion = "";
      state.selectedDevice = "";
      state.ready = false;
      status.textContent = "Firmware version index is not available yet; installation is disabled.";
    }

    state.selectedDevice = getCurrentDevices()[0]?.id || "";
    render();
  }

  function getCurrentVersion() {
    return state.versions.find((item) => item.version === state.selectedVersion) || state.versions[0];
  }

  function getCurrentDevices() {
    return getCurrentVersion()?.devices || [];
  }

  function getSelectedDevice() {
    return getCurrentDevices().find((device) => device.id === state.selectedDevice) || getCurrentDevices()[0];
  }

  function getVisibleDevices() {
    const query = state.filter.trim().toLowerCase();
    const devices = getCurrentDevices();
    if (!query) {
      return devices;
    }

    return devices.filter((device) =>
      `${device.name} ${device.id} ${device.yaml}`.toLowerCase().includes(query)
    );
  }

  function chooseVersion(version) {
    state.selectedVersion = version;
    state.selectedDevice = getCurrentDevices()[0]?.id || "";
    render();
  }

  function chooseDevice(deviceId) {
    state.selectedDevice = deviceId;
    render();
  }

  function renderVersionSelect() {
    versionSelect.replaceChildren();
    for (const item of state.versions) {
      const option = document.createElement("option");
      option.value = item.version;
      option.textContent = item.version;
      option.selected = item.version === state.selectedVersion;
      versionSelect.append(option);
    }
    versionSelect.value = state.selectedVersion;
    versionSelect.disabled = !state.ready;
  }

  function renderDeviceSelect() {
    deviceSelect.replaceChildren();
    for (const device of getCurrentDevices()) {
      const option = document.createElement("option");
      option.value = device.id;
      option.textContent = device.name;
      option.selected = device.id === state.selectedDevice;
      deviceSelect.append(option);
    }
    deviceSelect.value = state.selectedDevice;
    deviceSelect.disabled = !state.ready;
  }

  function renderGrid() {
    grid.replaceChildren();
    const visibleDevices = getVisibleDevices();

    if (visibleDevices.length === 0) {
      const empty = document.createElement("p");
      empty.className = "empty";
      empty.textContent = state.ready ? "No matching devices." : "Firmware index is not available.";
      grid.append(empty);
      return;
    }

    for (const device of visibleDevices) {
      const button = document.createElement("button");
      button.type = "button";
      button.className = "device-card";
      button.setAttribute("aria-pressed", String(device.id === state.selectedDevice));
      button.innerHTML = '<span class="device-name"></span><span class="device-meta"></span>';
      button.querySelector(".device-name").textContent = device.name;
      button.querySelector(".device-meta").textContent = device.yaml;
      button.addEventListener("click", () => chooseDevice(device.id));
      grid.append(button);
    }
  }

  function renderInstaller() {
    const version = getCurrentVersion();
    const device = getSelectedDevice();

    installer.setAttribute("aria-disabled", String(!state.ready));
    search.disabled = !state.ready;

    if (!version || !device) {
      title.textContent = "No firmware available";
      detail.textContent = "Firmware index is unavailable; installation is disabled.";
      installButton.removeAttribute("manifest");
      installButton.setAttribute("disabled", "");
      return;
    }

    title.textContent = `${device.name} - ${version.version}`;
    detail.textContent = `${device.yaml} -> ${device.manifest}`;
    installButton.setAttribute("manifest", device.manifest);
    installButton.removeAttribute("disabled");
  }

  function render() {
    renderVersionSelect();
    renderDeviceSelect();
    renderGrid();
    renderInstaller();
  }

  versionSelect.addEventListener("change", (event) => chooseVersion(event.target.value));
  deviceSelect.addEventListener("change", (event) => chooseDevice(event.target.value));
  search.addEventListener("input", (event) => {
    state.filter = event.target.value;
    renderGrid();
  });

  loadInstallerIndex();
</script>
