#!/usr/bin/env groovy
//@Library('k8s-ci-lib') _
@Grab('org.apache.commons:commons-math3:3.4.1')
import java.util.ServiceLoader;
import org.apache.commons.math3.primes.Primes

ServiceLoader.load(Primes.class);

void parallelize(int count) {
    if (!Primes.isPrime(count)) {
        error "${count} was not prime"
    }
    // …
}
pipeline {
    agent {
        label 'master'
    }
    options {
        // timestamps()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }
    environment {
        BUCKET_NAME = "optiva-uck-helm-charts"
    }
    parameters {
        booleanParam(name: 'MANUAL_BUILD', defaultValue: false, description: 'This is to disable autobuilds')
    }
    stages {
        stage("Disable autobuilds") {
            when {
                expression { !params.MANUAL_BUILD }
            }
            steps {
                sh "false"
            }
        }
        stage("Preparations") {
            parallelize(1)
        }
    }
}
