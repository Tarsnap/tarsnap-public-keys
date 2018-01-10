all: tarsnap-archive-keyring.gpg tarsnap-code-signing-keyring.gpg tarsnap-experimental-keyring.gpg

GPG=gpg --no-default-keyring --no-auto-check-trustdb

# Create the keyrings on the user's system, since the standard
# ascii armoured export format is more stable than keyring files.
tarsnap-archive-keyring.gpg:
	$(GPG)	--keyring ./tarsnap-archive-keyring-temp.gpg		\
		--import keys-packaging/tarsnap-deb-packaging-key-*.asc
	$(GPG)	--keyring ./tarsnap-archive-keyring-temp.gpg		\
		--output $@ --export
	rm tarsnap-archive-keyring-temp.gpg tarsnap-archive-keyring-temp.gpg~

tarsnap-experimental-keyring.gpg:
	$(GPG)	--keyring ./tarsnap-experimental-keyring-temp.gpg	\
		--import keys-packaging/tarsnap-EXPERIMENTAL-deb-packaging-key-*.asc
	$(GPG)	--keyring ./tarsnap-experimental-keyring-temp.gpg	\
		--output $@ --export
	rm tarsnap-experimental-keyring-temp.gpg			\
		tarsnap-experimental-keyring-temp.gpg~

tarsnap-code-signing-keyring.gpg:
	$(GPG)	--keyring ./tarsnap-code-signing-keyring-temp.gpg	\
		--import keys-signing/*.asc
	$(GPG)	--keyring ./tarsnap-code-signing-keyring-temp.gpg	\
		--output $@ --export
	rm tarsnap-code-signing-keyring-temp.gpg			\
		tarsnap-code-signing-keyring-temp.gpg~

# Don't install the code-signing or experimental keyrings to
# /etc/apt/trusted.gpg.d/
install: tarsnap-archive-keyring.gpg tarsnap-code-signing-keyring.gpg
	install -d $(DESTDIR)/etc/apt/trusted.gpg.d/
	install -d $(DESTDIR)/usr/share/keyrings/
	cp tarsnap-archive-keyring.gpg $(DESTDIR)/etc/apt/trusted.gpg.d/
	cp tarsnap-archive-keyring.gpg $(DESTDIR)/usr/share/keyrings/
	cp tarsnap-experimental-keyring.gpg $(DESTDIR)/usr/share/keyrings/
	cp tarsnap-code-signing-keyring.gpg $(DESTDIR)/usr/share/keyrings/

clean:
	rm -f tarsnap-*.gpg tarsnap-*.gpg~

.PHONY: install clean
