#!/usr/bin/env groovy

@library('KopyrinLib')
import org.barabuga.build

Boolean _buildMac = true
try {
	_buildMac = buildMac.toBoolean()
} catch (MissingPropertyException ignore) {
	_buildMac = true
}

Boolean _buildWin = true
try {
	_buildWin = buildWin.toBoolean()
} catch (MissingPropertyException ignore) {
	_buildWin = true
}

Boolean _buildUnix = true
try {
	_buildUnix = buildUnix.toBoolean()
} catch (MissingPropertyException ignore) {
	_buildUnix = true
}

timestamps {
	stage('build') {
		parallel(
			"build_win": {
				if (_buildWin)
					build.buildWin("windows", "build_win")
			},
			"build_mac": {
				if (_buildMac)
					build.buildUnix("mac", "build_mac")
			},
			"build_linux": {
				if (_buildUnix)
					build.buildUnix("linux", "build_unix")
			}
		)
	}
	
	stage('store') {
		node("master") {
			if (_buildMac)
				unstash "build_mac"
				
			if (_buildUnix)
				unstash "build_unix"
			
			if (_buildWin)
				unstash "build_win"

			archiveArtifacts artifacts: '*'
			deleteDir()
		}
	}
}