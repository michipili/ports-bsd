# Created by: ijliao
# $FreeBSD: head/graphics/ocaml-images/Makefile 368571 2014-09-19 15:45:14Z tijl $

PORTNAME=	images
PORTVERSION=	4.0.1
PORTREVISION=	1
PORTEPOCH=	2
CATEGORIES=	graphics
MASTER_SITES=	https://bitbucket.org/camlspotter/camlimages/get/
PKGNAMEPREFIX=	ocaml-
DISTNAME=	v${PORTVERSION}
DIST_SUBDIR=	ocaml-images

DIST_SUBDIR=	ocaml-images
DISTNAME=	v${PORTVERSION}
WRKSRC =	${WRKDIR}/camlspotter-camlimages-c803efa9d5d3

MAINTAINER=	michipili@gmail.com
COMMENT=	Objective Caml image processing library

BUILD_DEPENDS=	${LOCALBASE}/share/aclocal/ocaml.m4:${PORTSDIR}/lang/ocaml-autoconf
BUILD_DEPENDS+=	omake:${PORTSDIR}/devel/omake

USE_OCAML=		yes
USE_OCAML_FINDLIB=	yes
OCAMLFIND_DESTDIR=	${STAGEDIR}${PREFIX}/${OCAML_SITELIBDIR}

OCAML_PKGDIRS=	camlimages
OCAML_LDLIBS=	${OCAML_SITELIBDIR}/camlimages

OMAKESUBS+=	-e s@%%INCLUDESPORTS%%@${LOCALBASE}/include@
OMAKESUBS+=	-e s@%%INCLUDESX11%%@${LOCALBASE}/include/X11@
OMAKESUBS+=	-e s@%%INCLUDESPNG%%@${LOCALBASE}/include/libpng15@
OMAKESUBS+=	-e s@%%LDFLAGSPORTS%%@-L${LOCALBASE}/lib@

OMAKE=		omake 'PREFIX=${STAGEDIR}${PREFIX}'
OMARGS=		--dotomake .omake --force-dotomake
WRKSRC=		${WRKDIR}/camlspotter-camlimages-c803efa9d5d3

MAKE_JOBS_UNSAFE= yes

OPTIONS_DEFINE=	PNG JPEG TIFF XPM GIF FREETYPE GHOSTSCRIPT GTK2 DOCS
OPTIONS_DEFAULT=PNG JPEG TIFF XPM GIF FREETYPE GHOSTSCRIPT
OPTIONS_SUB=	yes

PNG_CONFIGURE_WITH=	png
PNG_LIB_DEPENDS=	libpng15.so:${PORTSDIR}/graphics/png

JPEG_CONFIGURE_WITH=	jpeg
JPEG_LIB_DEPENDS=	libjpeg.so:${PORTSDIR}/graphics/jpeg

TIFF_CONFIGURE_WITH=	tiff
TIFF_LIB_DEPENDS=	libtiff.so:${PORTSDIR}/graphics/tiff

XPM_CONFIGURE_WITH=	xpm

GIF_CONFIGURE_WITH=	gif
GIF_LIB_DEPENDS=	libgif.so:${PORTSDIR}/graphics/giflib

FREETYPE_CONFIGURE_WITH=freetype
FREETYPE_LIB_DEPENDS=	libttf.so:${PORTSDIR}/print/freetype

GHOSTSCRIPT_CONFIGURE_WITH= gs

GTK2_CONFIGURE_WITH=	liblgtk2
GTK2_BUILD_DEPENDS=	lablgtk2:${PORTSDIR}/x11-toolkits/ocaml-lablgtk2
GTK2_RUN_DEPENDS=	lablgtk2:${PORTSDIR}/x11-toolkits/ocaml-lablgtk2

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MXPM}
USE_XORG=	xpm
.endif

.if defined(WITH_GS)
USE_GHOSTSCRIPT=	yes
.endif

do-configure:
	@(cd ${WRKSRC} && ${REINPLACE_CMD} ${OMAKESUBS} OMakefile)
	(cd ${WRKSRC} && ${OMAKE} ${OMARGS} configure)

do-build:
	(cd ${WRKSRC} && ${OMAKE} ${OMARGS})

ocaml-findlib:
	# Overriding ocaml-findlib target which does not support stage
	# The consequence is that pkg-plist must be manually supported

do-install:
	@${MKDIR} ${STAGEDIR}${PREFIX}/${OCAML_LDLIBS}	
	(cd ${WRKSRC} && ${OMAKE} ${OMARGS} install)

.include <bsd.port.mk>
