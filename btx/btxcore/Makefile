all:
	git clone https://github.com/BTXexplorer/btxcore
	cd btxcore && git checkout master
	cd btxcore && rm -rf .git # Delete .git folder to reduce space
	cd btxcore && npm install --production
	rm -rf btxcore/node_modules/btxcore-node/bin/*.gz
	rm -rf btxcore/node_modules/btxcore-node/bin/bitcore-*/bin/btx-qt
	rm -rf btxcore/node_modules/btxcore-node/bin/bitcore-*/bin/test_btx
	rm -rf btxcore/node_modules/btxcore-node/test/*

clean:
	rm -fr btxcore
