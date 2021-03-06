# $NetBSD: Makefile,v 1.31 2015/08/22 23:49:13 wiz Exp $

.include "../../base/go/version.mk"

DISTNAME=	go${GO15_VERSION}.src
PKGNAME=	go-${GO15_VERSION}
PKGREVISION=	1
CATEGORIES=	lang
MASTER_SITES=	https://storage.googleapis.com/golang/

MAINTAINER=	bsiegert@NetBSD.org
HOMEPAGE=	http://golang.org/
COMMENT=	The Go programming language
LICENSE=	modified-bsd

WRKSRC=		${WRKDIR}/go
USE_TOOLS+=	bash:run perl:run pax

# uses ulimit -T
BUILD_DEPENDS+=	bash>=4.2nb3:../../shells/bash

# cgo compiles under TMPDIR
TMPDIR?=	/tmp
BUILDLINK_PASSTHRU_DIRS+= ${TMPDIR}

# bash script may use unportable bash constructs
CHECK_PORTABILITY_SKIP=	lib/time/update.bash
CHECK_PORTABILITY_SKIP=	misc/ios/clangwrap.sh

GOROOT_FINAL=	${PREFIX}/go
INSTALLATION_DIRS= bin

REPLACE_BASH+=	doc/articles/wiki/test.bash
REPLACE_BASH+=	doc/codewalk/run
REPLACE_BASH+=	lib/time/update.bash
REPLACE_BASH+=	misc/arm/a
REPLACE_BASH+=	misc/benchcmp
REPLACE_BASH+=	misc/cgo/errors/test.bash
REPLACE_BASH+=	misc/cgo/testcarchive/test.bash
REPLACE_BASH+=	misc/cgo/testcshared/test.bash
REPLACE_BASH+=	misc/cgo/testgodefs/test.bash
REPLACE_BASH+=	misc/nacl/go_nacl_386_exec
REPLACE_BASH+=	misc/nacl/go_nacl_amd64p32_exec
REPLACE_BASH+=	misc/nacl/go_nacl_arm_exec
REPLACE_BASH+=	src/all.bash
REPLACE_BASH+=	src/androidtest.bash
REPLACE_BASH+=	src/bootstrap.bash
REPLACE_BASH+=	src/buildall.bash
REPLACE_BASH+=	src/clean.bash
REPLACE_BASH+=	src/cmd/compile/internal/big/vendor.bash
REPLACE_BASH+=	src/cmd/go/mkalldocs.sh
REPLACE_BASH+=	src/iostest.bash
REPLACE_BASH+=	src/make.bash
REPLACE_BASH+=	src/nacltest.bash
REPLACE_BASH+=	src/race.bash
REPLACE_BASH+=	src/run.bash
REPLACE_BASH+=	src/runtime/mknacl.sh
REPLACE_BASH+=	src/syscall/mkall.sh
REPLACE_BASH+=	src/syscall/mkerrors.sh
REPLACE_BASH+=	src/syscall/mksysnum_plan9.sh
REPLACE_BASH+=	test/bench/shootout/timing.sh

REPLACE_PERL+=	src/net/http/cgi/testdata/test.cgi
REPLACE_PERL+=	src/regexp/syntax/make_perl_groups.pl
REPLACE_PERL+=	src/syscall/*.pl
REPLACE_PERL+=	test/errchk

# uses /bin/rc (for Plan 9)
CHECK_INTERPRETER_SKIP+=	go/include/plan9/mklibc.rc
CHECK_INTERPRETER_SKIP+=	go/src/all.rc
CHECK_INTERPRETER_SKIP+=	go/src/clean.rc
CHECK_INTERPRETER_SKIP+=	go/src/make.rc
CHECK_INTERPRETER_SKIP+=	go/src/run.rc

BUILD_DEPENDS+=		go-1.4*:../../base/go14
GOROOT_BOOTSTRAP=	${PREFIX}/go14

do-build:
	cd ${WRKSRC}/src && env GOROOT_BOOTSTRAP=${GOROOT_BOOTSTRAP:Q} GOROOT_FINAL=${GOROOT_FINAL:Q} ${BASH} ./make.bash

do-install:
	cd ${WRKSRC} && rm -rf .hgignore .hgtags pkg/obj
	cd ${WRKDIR} && pax -rw go ${DESTDIR}${PREFIX}
	find ${DESTDIR}${PREFIX}/go -name \*.orig -exec rm {} \;
.for cmd in go gofmt
	cd ${DESTDIR}${PREFIX}/bin && ln -sf ../go/bin/${cmd} ${cmd}
.endfor

.include "../../mk/bsd.pkg.mk"
