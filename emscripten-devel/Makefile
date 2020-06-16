PORTNAME=emscripten-devel
DISTVERSION=1.39.18
CATEGORIES=devel
MAINTAINER=kpedersen@gmx.com
COMMENT=Toolchain for compiling to asm.js and WebAssembly

LICENSE=APACHE20 MIT NCSA
LICENSE_COMB=multi

BINARYEN_COMMIT=d26f90b
EMSCRIPTEN_COMMIT=d9b71c8
LLVM_COMMIT=f8170d8

USES=python

BUILD_DEPENDS= \
	${LOCALBASE}/bin/cmake:devel/cmake

RUN_DEPENDS= \
	${LOCALBASE}/bin/node:www/node

USE_GITHUB=yes
GH_TUPLE= \
	WebAssembly:binaryen:$(BINARYEN_COMMIT):BINARYEN \
	llvm:llvm-project:$(LLVM_COMMIT):LLVM \
	emscripten-core:emscripten:$(EMSCRIPTEN_COMMIT)

do-configure:
	cd ${WRKSRC_BINARYEN} && mkdir build && cd build && \
		cmake .. \
		-DCMAKE_BUILD_TYPE=Release \
		-DCMAKE_INSTALL_PREFIX=${STAGEDIR}${PREFIX}/lib/${PORTNAME}/binaryen

	cd ${WRKSRC_LLVM} && mkdir build && cd build && \
		cmake ../llvm \
		-DCMAKE_BUILD_TYPE=Release \
		-DCMAKE_INSTALL_PREFIX=${STAGEDIR}${PREFIX}/lib/${PORTNAME}/llvm \
		-DLLVM_ENABLE_PROJECTS='lld;clang' \
		-DLLVM_TARGETS_TO_BUILD="host;WebAssembly" \
		-DLLVM_INCLUDE_EXAMPLES=OFF -DLLVM_INCLUDE_TESTS=OFF

do-build:
	cd ${WRKSRC_BINARYEN}/build && \
		cmake --build .

	cd ${WRKSRC_LLVM}/build && \
		cmake --build .

do-install:
	cp -r ${WRKSRC} ${STAGEDIR}${PREFIX}/lib/${PORTNAME}
	@rm -r -f ${STAGEDIR}${PREFIX}/lib/${PORTNAME}/.* || true
	rm -r -f ${STAGEDIR}${PREFIX}/lib/${PORTNAME}/*.bat

	cat ${SCRIPTDIR}/emcc > ${STAGEDIR}${PREFIX}/bin/emcc
	chmod ugo+x ${STAGEDIR}${PREFIX}/bin/emcc
	cd ${STAGEDIR}${PREFIX}/bin && \
		ln -s emcc em++ && \
		ln -s emcc emar && \
		ln -s emcc emcmake && \
		ln -s emcc emconfigure && \
		ln -s emcc emmake && \
		ln -s emcc emranlib && \
		ln -s emcc emrun && \
		ln -s emcc emscons && \
		ln -s emcc emsize

	cd ${WRKSRC_BINARYEN}/build && \
		cmake --build . --target install

	cd ${WRKSRC_LLVM}/build && \
		cmake --build . --target install

.include <bsd.port.mk>

