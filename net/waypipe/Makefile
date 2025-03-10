PORTNAME=	waypipe
DISTVERSIONPREFIX=	v
DISTVERSION=	0.9.2
PORTREVISION=	2
CATEGORIES=	net wayland

PATCH_SITES=	${GL_SITE}/${GL_ACCOUNT}/${GL_PROJECT}/-/commit/
PATCHFILES+=	eccbb5cbf1e3.patch:-p1 # https://gitlab.freedesktop.org/mstoeckl/waypipe/-/issues/110
PATCHFILES+=	4216e58e3c0f.patch:-p1 # https://gitlab.freedesktop.org/mstoeckl/waypipe/-/issues/116
PATCHFILES+=	853cfbb482be.patch:-p1 # https://gitlab.freedesktop.org/mstoeckl/waypipe/-/issues/115

MAINTAINER=	jbeich@FreeBSD.org
COMMENT=	Network transparency with Wayland
WWW=		https://gitlab.freedesktop.org/mstoeckl/waypipe

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/COPYING

LIB_DEPENDS=	libwayland-client.so:graphics/wayland

USES=		compiler:c11 localbase:ldflags meson pkgconfig
USE_GITLAB=	yes
GL_SITE=	https://gitlab.freedesktop.org
GL_ACCOUNT=	mstoeckl
PLIST_FILES=	bin/${PORTNAME}

OPTIONS_DEFINE=	FFMPEG LZ4 MANPAGES OPENGL VAAPI ZSTD
OPTIONS_DEFAULT=FFMPEG LZ4 MANPAGES OPENGL VAAPI ZSTD

FFMPEG_LIB_DEPENDS=	libavcodec.so:multimedia/ffmpeg
FFMPEG_MESON_ENABLED=	with_video

LZ4_LIB_DEPENDS=	liblz4.so:archivers/liblz4
LZ4_MESON_ENABLED=	with_lz4

MANPAGES_BUILD_DEPENDS=	scdoc:textproc/scdoc
MANPAGES_MESON_ENABLED=	man-pages
MANPAGES_PLIST_FILES=	share/man/man1/${PORTNAME}.1.gz

OPENGL_USES=		gl
OPENGL_USE=		GL=gbm
OPENGL_LIB_DEPENDS=	libdrm.so:graphics/libdrm
OPENGL_MESON_ENABLED=	with_dmabuf

VAAPI_LIB_DEPENDS=	libva.so:multimedia/libva
VAAPI_MESON_ENABLED=	with_vaapi
VAAPI_IMPLIES=		FFMPEG

ZSTD_LIB_DEPENDS=	libzstd.so:archivers/zstd
ZSTD_MESON_ENABLED=	with_zstd

post-patch:
# Extract (snapshot) version from the port instead of meson.build
	@${REINPLACE_CMD} -i .nogit -e 's/git.found()/false/' \
		-e '/project_version/s/@0@/${DISTVERSIONFULL}/' \
		${WRKSRC}/meson.build

.include <bsd.port.mk>
