# Created by: Stanislav Sedov <ssedov@mbsd.msk.ru>
# $FreeBSD: head/devel/omake/Makefile 327724 2013-09-20 17:13:42Z bapt $

PORTNAME=	omake
DISTVERSION=	0.9.8.6-0.rc1
CATEGORIES=	devel
MASTER_SITES=	http://main.metaprl.org/download/omake/

MAINTAINER=	bf@FreeBSD.org
COMMENT=	Flexible build system

WRKSRC=		${WRKDIR}/${PORTNAME}-${DISTVERSION:C|-.*||}
USE_OCAML=	yes
NO_OCAML_RUNDEPENDS=yes
USES=		ncurses readline
MAKE_JOBS_UNSAFE=	yes

INFO=		omake-doc

NO_STAGE=	yes
.include <bsd.port.pre.mk>

.if ${ARCH} == "powerpc"
BROKEN=		Does not build on powerpc
.endif

post-patch:
#
# Dirty hack for PREFIX safety
#
	@${REINPLACE_CMD} -E -e \
		's|(getenv[[:space:]]+)LIBDIR|\1PREFLIBDIR|' ${WRKSRC}/mk/defaults

#
# Permissions safety
#
.for f in OMakefile doc/OMakefile src/main/OMakefile
	@${REINPLACE_CMD} -e \
		's|cp -f -m 444|$$(BSD_INSTALL_DATA)| ; \
		 s|cp -f -m 555|$$(BSD_INSTALL_PROGRAM)|' ${WRKSRC}/${f}
.endfor
	@${REINPLACE_CMD} -e 's| -j2 | |' ${WRKSRC}/Makefile
.if ${NCURSESBASE} == "/usr"
.for f in lib/configure/ncurses.om src/libmojave-external/cutil/lm_terminfo.c
	@${REINPLACE_CMD} -E -e 's|ncurses/(term\.h)|\1|' ${WRKSRC}/${f}
.endfor
.endif

check regression-test test: build
	@cd ${WRKSRC}; ${SETENV} ${MAKE_ENV} ${MAKE_ARGS} boot/omake check
	@${FIND} -ds ${WRKSRC}/test -type f -name 'result.log' -print0 | \
		${XARGS} -0tI @ ${CAT} @

bench: build
	@cd ${WRKSRC}; ${SETENV} ${MAKE_ENV} ${MAKE_ARGS} boot/omake bench

post-install:
	@(cd ${WRKSRC}/doc/info \
		&& ${INSTALL_DATA} omake-doc.info ${PREFIX}/${INFO_PATH})
	@(cd ${WRKSRC}/doc/info \
		&& ${INSTALL_DATA} omake-doc.info-* ${PREFIX}/${INFO_PATH})
.if ${PORT_OPTIONS:MDOCS}
	@${MKDIR} ${DOCSDIR}
	@(cd ${WRKSRC}/doc/html && ${COPYTREE_SHARE} . ${DOCSDIR})
.endif

.include <bsd.port.post.mk>
