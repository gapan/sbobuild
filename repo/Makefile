VERSION=14.2

SSH_HOST=salixos.org
SSH_PORT=22
SSH_USER=repo

.PHONY: upload
upload:
	make upload-i486
	make upload-x86_64

.PHONY: upload-i486
upload-i486:
	rsync -av --exclude 'metagen' --exclude 'dummy' i486/pkg/salix $(SSH_USER)@$(SSH_HOST):repo/i486/extra-${VERSION}/
	rsync -av i486/log $(SSH_USER)@$(SSH_HOST):repo/i486/extra-${VERSION}/
	rsync -av i486/source $(SSH_USER)@$(SSH_HOST):repo/i486/extra-${VERSION}/

.PHONY: upload-x86_64
upload-x86_64:
	rsync -av --exclude 'metagen' --exclude 'dummy' x86_64/pkg/salix $(SSH_USER)@$(SSH_HOST):repo/x86_64/extra-${VERSION}/
	rsync -av x86_64/log $(SSH_USER)@$(SSH_HOST):repo/x86_64/extra-${VERSION}/
	rsync -av x86_64/source $(SSH_USER)@$(SSH_HOST):repo/x86_64/extra-${VERSION}/

.PHONY: clean
clean:
	make clean-i486
	make clean-x86_64

.PHONY: clean-i486
clean-i486:
	rm -rf i486
	cp -r empty i486

.PHONY: clean-x86_64
clean-x86_64:
	rm -rf x86_64
	cp -r empty x86_64

.PHONY: compare
compare:
	@find i486/pkg/salix -name "*.txz" | sed "s|.*salix/\(.*\)/\(.*\)-\(.*\)-\(.*\)-\(.*\)\.txz|\2|" | sort > PKG32
	@find x86_64/pkg/salix -name "*.txz" | sed "s|.*salix/\(.*\)/\(.*\)-\(.*\)-\(.*\)-\(.*\)\.txz|\2|" | sort > PKG64
	@diff -Naur PKG32 PKG64 | grep "^+\|^-" | less
	@rm -f PKG32 PKG64

.PHONY: changelog32
changelog32:
	@find i486/pkg/salix -name "*.txz" ! -name "dummy-*" | sed "s|^\.i486/pkg/salix/||" | sed "s/txz$$/txz: Added./" | sort

.PHONY: changelog64
changelog64:
	@find x86_64/pkg/salix -name "*.txz" ! -name "dummy-*" | sed "s|^\.x86_64/pkg/salix||" | sed "s/txz$$/txz: Added./" | sort
