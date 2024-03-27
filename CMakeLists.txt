#----------------------------------------
#
# Copyright (c) 2024 Phil Erwin
# SPDX-License-Identifier: Apache-2.0
#
#----------------------------------------
cmake_minimum_required(VERSION 3.20)

set(LORA_BASICS_ROOT       $ENV{ZEPHYR_BASE}/../SWSD004 )

set(APPS                   ${LORA_BASICS_ROOT}/apps )
set(LORA_BASICS_MODEM      ${LORA_BASICS_ROOT}/lora_basics_modem/lora_basics_modem )
set(SMTC_MODEM_CORE        ${LORA_BASICS_MODEM}/smtc_modem_core )
set(SMTC_MODEM_SERVICES    ${SMTC_MODEM_CORE}/smtc_modem_services )
set(LR1MAC                 ${SMTC_MODEM_CORE}/lr1mac )

set(GEOLOCATION_MIDDLEWARE ${LORA_BASICS_ROOT}/geolocation_middleware )
set(SMTC_HAL               ${LORA_BASICS_ROOT}/smtc_hal )
set(PRINTERS               ${LORA_BASICS_ROOT}/lora_basics_modem/printers )
set(SHIELDS                ${LORA_BASICS_ROOT}/shields )
set(NRF52840               ${LORA_BASICS_ROOT}/smtc_hal/Nordic/nRF52840 )

set(FIRMWARE_UPDATE_ROOT   $ENV{ZEPHYR_BASE}/../SWTL001 )

set(CONNECTIVITY_TECHNOLOGY $ENV{ZEPHYR_BASE}/../connectivity-technology )


set(ZEPHYR_MODULES
  $ENV{ZEPHYR_BASE}/../modules/hal/cmsis
  $ENV{ZEPHYR_BASE}/../modules/hal/nordic
  )

set(EXTRA_ZEPHYR_MODULES
  ${CONNECTIVITY_TECHNOLOGY}
  ${LORA_BASICS_ROOT}
  ${FIRMWARE_UPDATE_ROOT}
  #	$ENV{ZEPHYR_BASE}/../nrf

  $ENV{ZEPHYR_BASE}/../modules/debug/segger
  $ENV{ZEPHYR_BASE}/../modules/crypto/tinycrypt
  #	$ENV{ZEPHYR_BASE}/../nrfxlib

#   $ENV{ZEPHYR_BASE}/../modules/hal/nxp
  $ENV{ZEPHYR_BASE}/../modules/lib/loramac-node
  )

find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})

project(wio_wm1110)

zephyr_compile_definitions( REGION_US_915=1 )
zephyr_compile_definitions( LR11XX=1 )
zephyr_compile_definitions( LR11XX_TRANSCEIVER=1 )
zephyr_compile_definitions( MODEM_HAL_DBG_TRACE=0 )
zephyr_compile_definitions( TASK_EXTENDED_2 )

zephyr_compile_options( -Wall -Werror )

target_sources(app PRIVATE src/main.c)

zephyr_include_directories( ${APPS}/common )

zephyr_include_directories( ${CONNECTIVITY_TECHNOLOGY}/src/pe4259 )

zephyr_include_directories( ${GEOLOCATION_MIDDLEWARE}/common )
zephyr_include_directories( ${GEOLOCATION_MIDDLEWARE}/gnss/src )
zephyr_include_directories( ${GEOLOCATION_MIDDLEWARE}/wifi/src )


zephyr_include_directories( ${LORA_BASICS_MODEM} )
zephyr_include_directories( ${LORA_BASICS_MODEM}/smtc_modem_api )
zephyr_include_directories( ${LORA_BASICS_MODEM}/smtc_modem_core/radio_drivers/lr11xx_driver/src )
zephyr_include_directories( ${LORA_BASICS_MODEM}/smtc_modem_core/radio_planner/src )
zephyr_include_directories( ${LORA_BASICS_MODEM}/smtc_modem_core/smtc_ral/src )
zephyr_include_directories( ${LORA_BASICS_MODEM}/smtc_modem_core/smtc_ralf/src )
zephyr_include_directories( ${LORA_BASICS_MODEM}/smtc_modem_core/smtc_modem_crypto )
zephyr_include_directories( ${LORA_BASICS_MODEM}/smtc_modem_core/smtc_modem_crypto/smtc_secure_element )
zephyr_include_directories( ${LORA_BASICS_MODEM}/smtc_modem_hal )
zephyr_include_directories( ${LORA_BASICS_MODEM}/utilities/user_app/smtc_hal_l4 )

zephyr_include_directories( ${LR1MAC} )
zephyr_include_directories( ${LR1MAC}/src )
zephyr_include_directories( ${LR1MAC}/src/lr1mac_class_b )
zephyr_include_directories( ${LR1MAC}/src/lr1mac_class_c )
zephyr_include_directories( ${LR1MAC}/src/services )
zephyr_include_directories( ${LR1MAC}/src/smtc_real/src )

zephyr_include_directories( ${NRF52840}/inc )  # smtc_hal_flash_mapping.h

zephyr_include_directories( ${PRINTERS} )

zephyr_include_directories( ${SHIELDS} )
zephyr_include_directories( ${SHIELDS}/interface )
zephyr_include_directories( ${SHIELDS}/LR11XX/radio_drivers_hal )
zephyr_include_directories( ${SHIELDS}/LR11XX/smtc_shield_lr11xx/common/inc )
zephyr_include_directories( ${SHIELDS}/LR11XX/LR1110TRK1xKS/BSP/board )
zephyr_include_directories( ${SHIELDS}/LR11XX/LR1110TRK1xKS/BSP/peripherals/Leds )
zephyr_include_directories( ${SHIELDS}/LR11XX/LR1110TRK1xKS/BSP/peripherals/hall_effect )
zephyr_include_directories( ${SHIELDS}/LR11XX/LR1110TRK1xKS/BSP/peripherals/usr_button )

zephyr_include_directories( ${SMTC_HAL}/board )
zephyr_include_directories( ${SMTC_HAL}/inc )

zephyr_include_directories( ${SMTC_MODEM_CORE}/device_management )
zephyr_include_directories( ${SMTC_MODEM_CORE}/lorawan_api )
zephyr_include_directories( ${SMTC_MODEM_CORE}/modem_config )
zephyr_include_directories( ${SMTC_MODEM_CORE}/modem_services )
zephyr_include_directories( ${SMTC_MODEM_CORE}/modem_supervisor )

zephyr_include_directories( ${SMTC_MODEM_SERVICES} )
zephyr_include_directories( ${SMTC_MODEM_SERVICES}/headers )
zephyr_include_directories( ${SMTC_MODEM_SERVICES}/src )
zephyr_include_directories( ${SMTC_MODEM_SERVICES}/src/stream )

zephyr_include_directories( ${FIRMWARE_UPDATE_ROOT}/application/inc )
zephyr_include_directories( ${FIRMWARE_UPDATE_ROOT}/lr1110_modem_driver/src )
