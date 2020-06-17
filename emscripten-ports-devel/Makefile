PORTNAME=emscripten-ports
PKGNAMESUFFIX=-devel
DISTVERSION=1.39.18
CATEGORIES=devel
MAINTAINER=kpedersen@gmx.com
COMMENT=Emscripten ported libraries

LICENSE=APACHE20 MIT NCSA
LICENSE_COMB=multi

SDL2_COMMIT=865eadd
ZLIB_COMMIT=69f24a2
#LIBPNG_COMMIT=918d23f

BUILD_DEPENDS= \
	${LOCALBASE}/bin/emcc:devel/emscripten-devel

RUN_DEPENDS= \
	${LOCALBASE}/bin/emcc:devel/emscripten-devel

USE_GITHUB=yes
GH_TUPLE= \
	emscripten-ports:SDL2:$(SDL2_COMMIT) \
	emscripten-ports:zlib:$(ZLIB_COMMIT):ZLIB
#	emscripten-ports:libpng:$(LIBPNG_COMMIT):LIBPNG

do-configure:
	cd ${WRKSRC} && mkdir build && cd build && \
		emcmake cmake .. \
		-DCMAKE_BUILD_TYPE=Release \
		-DCMAKE_INSTALL_PREFIX=${STAGEDIR}${PREFIX}/lib/emscripten/system/local

	cd ${WRKSRC_ZLIB} && mkdir build && cd build && \
		emcmake cmake .. \
		-DCMAKE_BUILD_TYPE=Release \
		-DCMAKE_INSTALL_PREFIX=${STAGEDIR}${PREFIX}/lib/emscripten/system/local

#	cd ${WRKSRC_LIBPNG} && mkdir -p build && cd build && \
#		emcmake cmake .. \
#		-DCMAKE_BUILD_TYPE=Release \
#		-DCMAKE_INSTALL_PREFIX=${STAGEDIR}${PREFIX}/lib/emscripten/system/local

do-build:
	cd ${WRKSRC}/build && \
		cmake --build .

	cd ${WRKSRC_ZLIB}/build && \
		cmake --build .

#	cd ${WRKSRC_LIBPNG}/build && \
#		cmake --build .

do-install:
	cd ${WRKSRC}/build && \
		cmake --build . --target install

	cd ${WRKSRC_ZLIB}/build && \
		cmake --build . --target install

#	cd ${WRKSRC_LIBPNG}/build && \
#		cmake --build . --target install

.include <bsd.port.mk>
