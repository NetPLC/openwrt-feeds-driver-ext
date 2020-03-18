SRC_DIR	:= ./bluetooth_usb_driver
MDL_DIR	:= /lib/modules/$(shell uname -r)
DRV_DIR	:= $(MDL_DIR)/kernel/drivers/bluetooth

FIRMWARE_DIR := 8723DU
##########################################
install:
	make -C $(FIRMWARE_DIR) -s

	- rmmod btusb
	- mv $(DRV_DIR)/btusb.ko $(DRV_DIR)/btusb_bak
	- rmmod rtk_btusb
	make -C $(SRC_DIR)
	cp -f $(SRC_DIR)/rtk_btusb.ko $(DRV_DIR)/rtk_btusb.ko
	depmod -a $(MDL_DIR)
	make -C $(SRC_DIR) clean
	echo "install rtk_btusb success!"

##########################################

uninstall:
	make -C $(FIRMWARE_DIR) clean -s

	- mv -n $(DRV_DIR)/btusb_bak $(DRV_DIR)/btusb.ko
	- rmmod rtk_btusb
	rm -f $(DRV_DIR)/rtk_btusb.ko
	depmod -a $(MDL_DIR)
	echo "uninstall rtk_btusb success!"
##########################################

