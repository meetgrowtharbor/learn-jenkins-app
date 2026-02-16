stage('Deploy') {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }
    steps {
        sh '''
            apk add --no-cache bash curl
            npm install netlify-cli
            
            # First, check what's configured in Netlify
            echo "Checking Netlify site configuration..."
            node_modules/.bin/netlify sites:info --site=$NETLIFY_SITE_ID --auth=$NETLIFY_AUTH_TOKEN
            
            # Override the build command using environment variable
            export NETLIFY_BUILD_COMMAND="echo 'Build skipped - using pre-built files'"
            
            echo "Deploying to Netlify site ID: $NETLIFY_SITE_ID"
            node_modules/.bin/netlify status
            
            # Deploy with explicit build command override
            node_modules/.bin/netlify deploy \
                --dir=build \
                --prod \
                --site=$NETLIFY_SITE_ID \
                --auth=$NETLIFY_AUTH_TOKEN \
                --build-command="echo 'Build skipped'" \
                --skip-functions-cache
        '''
    }
}