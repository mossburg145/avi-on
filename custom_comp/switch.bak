
"""Support for Avion ###."""
import importlib
import logging
import time

import voluptuous as vol

from homeassistant.components.switch import SwitchEntity

from homeassistant.const import (
    CONF_API_KEY,
    CONF_DEVICES,
    CONF_ID,
    CONF_NAME,
    CONF_PASSWORD,
    CONF_USERNAME,
)
import homeassistant.helpers.config_validation as cv

_LOGGER = logging.getLogger(__name__)

DEVICE_SCHEMA = vol.Schema(
    {
        vol.Required(CONF_API_KEY): cv.string,
        vol.Optional(CONF_ID): cv.positive_int,
        vol.Optional(CONF_NAME): cv.string,
    }
)

PLATFORM_SCHEMA = PLATFORM_SCHEMA.extend(
    {
        vol.Optional(CONF_DEVICES, default={}): {cv.string: DEVICE_SCHEMA},
        vol.Optional(CONF_USERNAME): cv.string,
        vol.Optional(CONF_PASSWORD): cv.string,
    }
)


def setup_platform(hass, config, add_entities, discovery_info=None):
    """Set up an Avion switch."""
    # pylint: disable=no-member
    avion = importlib.import_module("avion")

    lights = []
    if CONF_USERNAME in config and CONF_PASSWORD in config:
        devices = avion.get_devices(config[CONF_USERNAME], config[CONF_PASSWORD])
        for device in devices:
            lights.append(AvionLight(device))

    for address, device_config in config[CONF_DEVICES].items():
        device = avion.Avion(
            mac=address,
            passphrase=device_config[CONF_API_KEY],
            name=device_config.get(CONF_NAME),
            object_id=device_config.get(CONF_ID),
            connect=False,
        )
        
