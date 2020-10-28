deb-pkg
=======

I normally generate .deb packages out of my projects.

To do so, I keep a *debian/* directory on the master branch, with all the
relevant files.

Debian packaging rules do not like this, so normally I have a make target
`deb-pkg` that looks more or less like this one:

```
deb-pkg: dist
	mv typedload_`./setup.py --version`.orig.tar.gz* /tmp
	cd /tmp; tar -xf typedload_*.orig.tar.gz
	cp -r debian /tmp/typedload/
	cd /tmp/typedload/; dpkg-buildpackage --changes-option=-S
	mkdir deb-pkg
	mv /tmp/typedload_* /tmp/python3-typedload_*.deb deb-pkg
	$(RM) -r /tmp/typedload
```

It is a bit wasteful to do copy-paste of this in every project, and also
I can't do improvements, since it is all copy paste.

So instead I want to do this:


```
deb-pkg: dist
	deb-pkg projectname
```

And that's about it.
