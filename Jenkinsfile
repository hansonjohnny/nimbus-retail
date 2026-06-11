@Library('jenkins-shared-library') _

// ─────────────────────────────────────────
// Service pipeline configuration
// ecrCredId must match the Jenkins credential
// ID that holds the ECR repository name.
// ─────────────────────────────────────────
def serviceConfigs = [
    'auth-service': [
        appDir:          'services/auth-service',
        sonarProjectKey: 'nimbus-auth-service',
        sonarProjectName:'nimbus-auth-service',
        ecrCredId:       'ECR_REPO_AUTH_SERVICE'
    ],
    'catalog-service': [
        appDir:          'services/catalog-service',
        sonarProjectKey: 'nimbus-catalog-service',
        sonarProjectName:'nimbus-catalog-service',
        ecrCredId:       'ECR_REPO_CATALOG_SERVICE'
    ],
    'cart-service': [
        appDir:          'services/cart-service',
        sonarProjectKey: 'nimbus-cart-service',
        sonarProjectName:'nimbus-cart-service',
        ecrCredId:       'ECR_REPO_CART_SERVICE'
    ],
    'order-service': [
        appDir:          'services/order-service',
        sonarProjectKey: 'nimbus-order-service',
        sonarProjectName:'nimbus-order-service',
        ecrCredId:       'ECR_REPO_ORDER_SERVICE'
    ],
    'notification-service': [
        appDir:          'services/notification-service',
        sonarProjectKey: 'nimbus-notification-service',
        sonarProjectName:'nimbus-notification-service',
        ecrCredId:       'ECR_REPO_NOTIFICATION_SERVICE'
    ],
    'frontend': [
        appDir:          'frontend',
        sonarProjectKey: 'nimbus-frontend',
        sonarProjectName:'nimbus-frontend',
        ecrCredId:       'ECR_REPO_FRONTEND'
    ]
]

// ─────────────────────────────────────────
// Detect which service changed.
// Pass a map so frontend (at root) and all
// backend services are handled in one call.
// ─────────────────────────────────────────
def changedService = detectChangedService([
    'auth-service'        : 'services/auth-service/',
    'catalog-service'     : 'services/catalog-service/',
    'cart-service'        : 'services/cart-service/',
    'order-service'       : 'services/order-service/',
    'notification-service': 'services/notification-service/',
    'frontend'            : 'frontend/'
])

if (!changedService) {
    echo "No service-specific changes detected. Skipping pipeline."
    currentBuild.result = 'NOT_BUILT'
    return
}

// ─────────────────────────────────────────
// Run the pipeline for the changed service
// ─────────────────────────────────────────
def cfg = serviceConfigs[changedService]
fullPipeline(
    service:          changedService,
    appDir:           cfg.appDir,
    sonarProjectKey:  cfg.sonarProjectKey,
    sonarProjectName: cfg.sonarProjectName,
    ecrCredId:        cfg.ecrCredId,
    awsRegion:        'us-east-2'
)
