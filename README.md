/**************************
 * boards/arm/stm32h7/nucleo-h743zi/src/stm32_bringup.c

 * 
 *this is for led blink in stm32h7
 *
 *
 * SPDX-License-Identifier: Apache-2.0
 *
 **************************/

/**************************
 * Included Files
 **************************/
#include <nuttx/config.h>
#include <sys/types.h>
#include <syslog.h>
#include <errno.h>
#include <arch/board/board.h>
#include <nuttx/fs/fs.h>
#include <nuttx/leds/userled.h>  // Include user LED header

#ifdef CONFIG_USBMONITOR
#include <nuttx/usb/usbmonitor.h>
#endif

#ifdef CONFIG_STM32H7_OTGFS
#include "stm32_usbhost.h"
#endif

#ifdef CONFIG_INPUT_BUTTONS
#include <nuttx/input/buttons.h>
#endif

#ifdef HAVE_RTC_DRIVER
#include <nuttx/timers/rtc.h>
#include "stm32_rtc.h"
#endif

#ifdef CONFIG_STM32_ROMFS
#include "stm32_romfs.h"
#endif

#ifdef CONFIG_STM32H7_IWDG
#include "stm32_wdg.h"
#endif

#ifdef CONFIG_RNDIS
#include <nuttx/usb/rndis.h>
#endif

#include "stm32_gpio.h"

/**************************
 * Private Functions
 **************************/

/**************************
 * Name: stm32_i2c_register
 *
 * Description:
 *   Register one I2C driver for the I2C tool.
 **************************/
#if defined(CONFIG_I2C) && defined(CONFIG_SYSTEM_I2CTOOL)
static void stm32_i2c_register(int bus)
{
    struct i2c_master_s *i2c;
    int ret;

    i2c = stm32_i2cbus_initialize(bus);
    if (i2c == NULL)
    {
        syslog(LOG_ERR, "ERROR: Failed to get I2C%d interface\n", bus);
    }
    else
    {
        ret = i2c_register(i2c, bus);
        if (ret < 0)
        {
            syslog(LOG_ERR, "ERROR: Failed to register I2C%d driver: %d\n", bus, ret);
            stm32_i2cbus_uninitialize(i2c);
        }
    }
}
#endif

/**************************
 * Name: stm32_i2ctool
 *
 * Description:
 *   Register I2C drivers for the I2C tool.
 **************************/
#if defined(CONFIG_I2C) && defined(CONFIG_SYSTEM_I2CTOOL)
static void stm32_i2ctool(void)
{
#ifdef CONFIG_STM32H7_I2C1
    stm32_i2c_register(1);
#endif
#ifdef CONFIG_STM32H7_I2C2
    stm32_i2c_register(2);
#endif
#ifdef CONFIG_STM32H7_I2C3
    stm32_i2c_register(3);
#endif
#ifdef CONFIG_STM32H7_I2C4
    stm32_i2c_register(4);
#endif
}
#endif

/**************************
 * Public Functions
 **************************/

/**************************
 * Name: stm32_bringup
 *
 * Description:
 *   Perform architecture-specific initialization.
 **************************/
int stm32_bringup(void)
{
    int ret = OK;

#if defined(CONFIG_I2C) && defined(CONFIG_SYSTEM_I2CTOOL)
    stm32_i2ctool();
#endif

#ifdef CONFIG_FS_PROCFS
    /* Mount the procfs file system */
    ret = nx_mount(NULL, STM32_PROCFS_MOUNTPOINT, "procfs", 0, NULL);
    if (ret < 0)
    {
        syslog(LOG_ERR, "ERROR: Failed to mount the PROC filesystem: %d\n", ret);
    }
#endif

#ifdef CONFIG_USERLED
    /* Register the LED driver */
    ret = userled_lower_initialize("/dev/userleds");
    if (ret < 0)
    {
        syslog(LOG_ERR, "ERROR: userled_lower_initialize() failed: %d\n", ret);
    }
#endif

#ifdef CONFIG_INPUT_BUTTONS
    /* Register the BUTTON driver */
    ret = btn_lower_initialize("/dev/buttons");
    if (ret < 0)
    {
        syslog(LOG_ERR, "ERROR: btn_lower_initialize() failed: %d\n", ret);
    }
#endif

#ifdef HAVE_USBHOST
    /* Initialize USB host operation */
    ret = stm32_usbhost_initialize();
    if (ret != OK)
    {
        syslog(LOG_ERR, "ERROR: Failed to initialize USB host: %d\n", ret);
    }
#endif

#ifdef HAVE_USBMONITOR
    /* Start the USB Monitor */
    ret = usbmonitor_start();
    if (ret != OK)
    {
        syslog(LOG_ERR, "ERROR: Failed to start USB monitor: %d\n", ret);
    }
#endif

#ifdef CONFIG_DEV_GPIO
    /* Register the GPIO driver */
    ret = stm32_gpio_initialize();
    if (ret < 0)
    {
        syslog(LOG_ERR, "ERROR: Failed to initialize GPIO Driver: %d\n", ret);
        return ret;
    }
#endif

    return OK;
}
