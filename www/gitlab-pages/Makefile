PORTNAME=	gitlab-pages
PORTVERSION=	16.0.1
PORTREVISION=	1
DISTVERSIONPREFIX=	v
CATEGORIES=	www
MASTER_SITES+=	https://gitlab.com/gitlab-org/gitlab-pages/-/raw/v${DISTVERSION}/
DISTFILES+=	go.mod

MAINTAINER=	mfechner@FreeBSD.org
COMMENT=	Official GitLab Pages daemon
WWW=		https://gitlab.com/gitlab-org/gitlab-pages

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE

USES=		go:modules
GO_MODULE=	gitlab.com/gitlab-org/gitlab-pages

USE_GITLAB=	yes
GL_ACCOUNT=	gitlab-org
# Find this here: https://gitlab.com/gitlab-org/gitlab-pages/-/tags/
GL_COMMIT=	8cda253ced465c7c9764332d7f5210ea3c976236

GO_BUILDFLAGS=	-ldflags="-X 'main.VERSION=${PORTVERSION}' -X 'main.REVISION=${GL_COMMIT}'"

USE_RC_SUBR=	gitlab_pages

USERS=		gitlab-pages
GROUPS=		gitlab-pages

post-patch:
	${REINPLACE_CMD} -e "s|%%PREFIX%%|${PREFIX}|" ${WRKSRC}/gitlab-pages.conf.sample

do-install:
	@${MKDIR} ${STAGEDIR}${DATADIR}
	${MKDIR} ${STAGEDIR}${DATADIR}/bin
	(cd ${WRKDIR}/bin/ && ${COPYTREE_BIN} .  ${STAGEDIR}${DATADIR}/bin)
.for x in gitlab-pages.conf.sample
	${INSTALL_DATA} ${WRKSRC}/${x} ${STAGEDIR}${DATADIR}/
.endfor

post-install:
	${FIND} -s ${STAGEDIR}${DATADIR} -not -type d | ${SORT} | \
		${SED} -e 's#^${STAGEDIR}${PREFIX}/##' | \
		${SED} -E -e '/sample$$/ s#^#@sample #; \
		s#${DATADIR_REL}/bin#@(,,555) ${DATADIR_REL}/bin#; ' >> ${TMPPLIST}
	cat ${TMPPLIST}

.include <bsd.port.mk>
