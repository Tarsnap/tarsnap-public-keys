all: tarsnap-archive-keyring.gpg tarsnap-code-signing-keyring.gpg

# Create the keyring on the user's system, since the standard
# ascii armoured export format is more stable than keyring files.
tarsnap-archive-keyring.gpg:
	gpg --no-default-keyring				\
		--keyring ./tarsnap-archive-keyring.gpg		\
		--import keys-packaging/*.asc

tarsnap-code-signing-keyring.gpg:
	gpg --no-default-keyring				\
		--keyring ./tarsnap-code-signing-keyring.gpg	\
		--import keys-signing/*.asc

# Don't install the code-signing keyring to /etc/apt/trusted.gpg.d/
install: tarsnap-archive-keyring.gpg tarsnap-code-signing-keyring.gpg
	install -d $(DESTDIR)/etc/apt/trusted.gpg.d/
	install -d $(DESTDIR)/usr/share/keyrings/
	cp tarsnap-archive-keyring.gpg $(DESTDIR)/etc/apt/trusted.gpg.d/
	cp tarsnap-archive-keyring.gpg $(DESTDIR)/usr/share/keyrings/
	cp tarsnap-code-signing-keyring.gpg $(DESTDIR)/usr/share/keyrings/

clean:
	rm -f tarsnap-*-keyring.gpg tarsnap-*-keyring.gpg~

.PHONY: install clean
