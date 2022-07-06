all: tarsnap-archive-keyring.gpg tarsnap-code-signing-keyring.gpg tarsnap-experimental-keyring.gpg

# gpg really wants to have a home directory; this can be problematic inside
# certain package-building systems, which might not have a valid user home
# directory within its chroot or jail.
TMP_GPGHOME=gpg-tmp
TMP_GPGHOME_SENTINEL=gpg-tmp/sentinel
GPG=gpg --homedir=$(TMP_GPGHOME) --no-default-keyring --no-auto-check-trustdb

# Use a sentinel file so that we only create $(TMP_GPGHOME) once.
$(TMP_GPGHOME_SENTINEL):
	mkdir -p $(TMP_GPGHOME)
	chmod 700 $(TMP_GPGHOME)
	touch $(TMP_GPGHOME_SENTINEL)

# Create the keyrings on the user's system, since the standard
# ascii armoured export format is more stable than keyring files.
tarsnap-archive-keyring.gpg: $(TMP_GPGHOME_SENTINEL)
	$(GPG)	--keyring ./tarsnap-archive-keyring-temp.gpg		\
		--import keys-packaging/tarsnap-deb-packaging-key-*.asc
	$(GPG)	--keyring ./tarsnap-archive-keyring-temp.gpg		\
		--output $@ --export
	rm tarsnap-archive-keyring-temp.gpg tarsnap-archive-keyring-temp.gpg~

tarsnap-experimental-keyring.gpg: $(TMP_GPGHOME_SENTINEL)
	$(GPG)	--keyring ./tarsnap-experimental-keyring-temp.gpg	\
		--import keys-packaging/tarsnap-EXPERIMENTAL-deb-packaging-key-*.asc
	$(GPG)	--keyring ./tarsnap-experimental-keyring-temp.gpg	\
		--output $@ --export
	rm tarsnap-experimental-keyring-temp.gpg			\
		tarsnap-experimental-keyring-temp.gpg~

tarsnap-code-signing-keyring.gpg: $(TMP_GPGHOME_SENTINEL)
	$(GPG)	--keyring ./tarsnap-code-signing-keyring-temp.gpg	\
		--import keys-signing/*.asc
	$(GPG)	--keyring ./tarsnap-code-signing-keyring-temp.gpg	\
		--output $@ --export
	rm tarsnap-code-signing-keyring-temp.gpg			\
		tarsnap-code-signing-keyring-temp.gpg~

install: tarsnap-archive-keyring.gpg tarsnap-code-signing-keyring.gpg
	install -d $(DESTDIR)/usr/share/keyrings/
	cp tarsnap-archive-keyring.gpg $(DESTDIR)/usr/share/keyrings/
	cp tarsnap-experimental-keyring.gpg $(DESTDIR)/usr/share/keyrings/
	cp tarsnap-code-signing-keyring.gpg $(DESTDIR)/usr/share/keyrings/

clean:
	rm -f tarsnap-*.gpg tarsnap-*.gpg~
	rm -rf $(TMP_GPGHOME)

.PHONY: install clean
