#!/usr/bin/env groovy

def buildUnix(label, stashName) {
	node(label) {
		checkout scm
		sh '''#!/bin/bash -le
			gcc hello.c -o hello_${label}
		'''
	}
}

timestamps {
	stage('build') {
		parallel(
			"build_win": {
				node("windows") {
					checkout scm
					String vsvars_bat = 'Microsoft Visual Studio 12.0\\VC\\vcvarsall.bat'
					bat """
						call "%ProgramFiles(X86)%\\${vsvars_bat}" x86
						cl.exe hello.c
					"""
					stash name: "build_win", includes: "*.exe"
				}
			},
			"build_mac": {
				buildUnix("mac")
			},
			"build_linux": {
				buildUnix("linux")
			}
		)
	}
	
	stage('store') {
	}
}